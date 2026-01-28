**Web Attacks — Part 2**

Open Redirection • Directory Traversal • XXE • Brute Force

The goal is to picture what actually happens to the app and server, recognize the log patterns fast,
confirm safely, and then close it out properly. I keep the same mental
flow: first see it in monitoring, pull exact requests from log
management, check what web security and endpoint security saw, then fix
and retest. I tag **MITRE ATT&CK** techniques and **NIST CSF** phases
inside the text so investigations look complete later.

------------------------------------------------------------------------

**Open Redirection**

An application takes a URL from a query parameter and performs a
redirect without checking if the destination is trusted. Typical
parameters are next, url, redirect, continue, or return_to. When a user
clicks a crafted link on our domain, the server responds with a normal
302 but the Location header points to an external site controlled by the
attacker. Users think they’re still dealing with us, then land on a
phishing page or malware delivery. In OAuth and SSO flows, sloppy
redirects can leak tokens if the redirect target isn’t strictly
validated.

**What this looks like in data:** SIEM monitoring shows surges of
301/302 responses on endpoints such as /login?next=... or
/redirect?url=.... In access logs, the query string carries a full
absolute URL, sometimes obfuscated with encodings: %2f for slashes,
mixed casing, or “localhost tricks” like http://127.0.0.1/, \[::1\], or
even decimal/hex IPs like http://2130706433/ and http://0x7f000001/.
Some payloads drop the scheme and rely on //attacker.tld to be treated
as an absolute host by the browser. You may also see chains such as
?next=https://ourdomain.tld/redirect?url=https://attacker.tld to sneak
through naive checks.

**How to prove safely:** pull one of the exact requests from logs,
decode the parameter, and replay it in a test environment. If the server
issues a 302 to an external host without any allowlist logic, it’s open.
For OAuth, verify that redirect_uri is validated against a strict
allowlist and that state is signed and checked to prevent flow hijack.

**What to fix and how to close:** block external redirect attempts at
the WAF/CDN as a seatbelt, then fix code to accept only relative paths
or to validate against a small allowlist of exact domains and paths.
Normalize and canonicalize the input before any check so encodings and
double encodings don’t bypass validation. Re-test with the same payloads
that worked before. This lines up with **MITRE ATT&CK
T1189/T1566/T1204** in the overall kill chain and sits under **NIST CSF
Detect → Respond → Recover** for tracking.

**Useful search ideas:** in Splunk, find 302s where a redirect param
contains an absolute URL:

index=web sourcetype=access_combined status=302 (uri_query="\*next=\*"
OR uri_query="\*url=\*")

\| rex field=uri_query
"(?\<redir\>(?:next\|url\|redirect\|continue)=\[^&\]+)"

\| eval redir=urldecode(redir)

\| search redir="\*http\*://\*"

\| table \_time clientip uri_path redir

------------------------------------------------------------------------

**Directory Traversal**

An application uses user input to build a filesystem path without
normalizing or restricting it. Adding ../ lets the request walk out of
the web root and read sensitive files that the application’s OS user can
access. On Linux, the common targets are /etc/passwd, /etc/shadow,
/etc/hosts, app configs, and private keys. On Windows,
C:\Windows\win.ini, IIS config folders, and web.config are classic
giveaways. This is a data exposure problem; by itself it doesn’t execute
code, but it leaks enough to aid a later attack.

**What this looks like in data:** monitoring lights up with traversal
signatures like ../ and the encoded form %2e%2e%2f. Attackers often
double-encode or pad with a null byte (%00) to bypass weak filters.
Access logs show parameters named file, path, template, page, or include
containing many ../ segments and well-known file names. Successful reads
often have very consistent response sizes, because sensitive files tend
to be similar length across attempts.

**How to prove safely:** in a test environment, replay one or two of the
logged requests and verify the response body contains real file content.
Don’t try this directly on production unless you’re formally authorized
and capturing evidence; use the logs to prove exposure existed and
narrow the window. If you need a quick indicator without touching prod,
compare response size patterns for known targets around the time of the
attack.

**What to fix and how to close:** deploy a quick WAF rule to block
traversal patterns on the specific route while the application is
patched. In code, normalize the path, reject any .. and its encoded
forms, and switch from free-form file paths to explicit allowlists or
IDs. On the platform side, ensure the application’s OS user can’t read
system or secret files in the first place (least privilege). Retest with
the exact payloads from the logs. This maps to **MITRE ATT&CK T1190**
(exploiting the web app) and the data read aligns with **T1005**; treat
it under **NIST CSF Detect → Respond → Recover**.

**Useful search ideas:** find suspicious traversal in GETs with encoded
../:

index=web sourcetype=access_combined (uri_query="\*%2e%2e%2f\*")

\| table \_time clientip method uri_path uri_query status bytes

To focus on likely Linux leaks:

index=web sourcetype=access_combined uri_query="\*etc%2fpasswd\*"

------------------------------------------------------------------------

**XML External Entity (XXE)**

An XML parser is configured to resolve external entities. An attacker
submits XML that defines an entity pointing to a local file, like
file:///etc/passwd, or a remote URL. The parser happily fetches it. If
the application echoes that content, sensitive data is exposed directly.
If the entity points to an internal URL, the server becomes a proxy and
makes HTTP requests into the private network (SSRF). If entity expansion
is left wide open, a tiny payload can blow up resources (DoS, the
“billion laughs” pattern).

**What this looks like in data:** requests to XML-handling endpoints
contain \<!DOCTYPE and \<!ENTITY\> in the body; sometimes these are
URL-encoded as %21DOCTYPE. Right after those requests, outbound proxy
logs show the app server making HTTP calls to odd destinations such as
cloud metadata endpoints (http://169.254.169.254), admin consoles, or
internal services that should never be reachable from outside. If the
application echoes the result of entity resolution, response bodies
suddenly include file content with obvious markers like “root:x:0:0:”
for /etc/passwd.

**How to prove safely:** extract one of the raw request bodies from logs
and read the DTD block; you’ll see entity names and SYSTEM URIs that
point to files or network locations. Align timestamps between the
inbound XML and any outbound calls in proxy logs to show SSRF. In a
controlled test environment, submit a harmless XML that tries to resolve
a benign external entity to confirm the parser’s behavior without
touching real secrets.

**What to fix and how to close:** disable external entity resolution in
the parser configuration. Many frameworks have a “secure” mode or
explicit flags to turn off DTDs and external entities; use them.
Validate inbound XML against strict schemas and reject requests carrying
DTDs entirely if your app doesn’t need them. Add a temporary WAF rule to
drop XML bodies that contain \<!DOCTYPE or \<!ENTITY\> while the fix
rolls out. If anything sensitive might have been exposed or tokens were
reachable internally, rotate credentials and review access logs for
abuse. This aligns with **MITRE ATT&CK T1190** (exploiting the web app),
SSRF over HTTP fits **T1071.001**, and data exfil via web aligns with
**T1567**; handle it under **NIST CSF Detect → Respond → Recover**.

**Useful search ideas:** spot incoming XML with DTDs and the follow-on
SSRF:

index=web sourcetype=app_xml (request_body="\*\<!DOCTYPE\*" OR
request_body="\*\<!ENTITY\*")

\| table \_time clientip uri_path status bytes

Correlate to outbound proxy in the same window:

index=proxy host=\<app_server\> (url="http://169.254.169.254/\*" OR
url="http\*://\*internal\*")

\| table \_time src dest url status bytes

------------------------------------------------------------------------

**Brute Force (web login)**

A client automates login attempts until it succeeds. Two common patterns
appear in the wild. The first hits a single user with many passwords
repeatedly. The second, called password spraying, uses a handful of very
common passwords against many different usernames to dodge lockout
controls. Once a valid combination lands, the attacker uses the
authenticated session like any other user.

**What this looks like in data:** authentication endpoints show bursts
of POSTs with status 401 or 403 from the same IP or a small pool of
addresses, followed by one 200 or 302 when a correct password is found.
Spray campaigns spread those failures across many accounts but reuse the
same few passwords, so you’ll see lots of distinct usernames combined
with identical request sizes and similar timing. Time of day and
geography often look off compared to normal users. If the site has weak
or no lockout, the same account will be hammered continuously.

**How to prove safely:** aggregate failed logins by username and by
source IP in short time windows (for example, 5–15 minutes). Look for
sequences that end with a success for that username. Then review the
successful account’s recent actions, IP history, and user-agent to see
if anything changed abruptly. For spraying, the signal is many different
usernames hit from the same IP with a small fixed set of passwords.

**What to fix and how to close:** enforce lockouts or exponential
backoff after several failures; rate-limit by IP and by username;
require MFA; and use WAF or bot protection to slow or block known
automation. Reset and notify any accounts that were both targeted and
then successfully logged in, and audit those sessions for sensitive
actions. This is squarely **MITRE ATT&CK T1110** and should be tracked
under **NIST CSF Detect → Respond → Recover**.

**Useful search ideas:** a quick look at failures and the first success:

index=web (uri_path="/login" OR uri_path="/auth")

\| stats count AS total by clientip, user, status

A slightly smarter approach:

index=web (uri_path="/login" OR uri_path="/auth")

\| eval outcome=if(status IN (200,302),"success","fail")

\| transaction user maxspan=15m keepevicted=true

\| search outcome="success" AND duration\>0 AND eventcount\>1

\| table \_time user clientip duration eventcount

------------------------------------------------------------------------

