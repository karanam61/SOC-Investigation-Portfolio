# Web Attacks — Part 1 (simple explanations)

Format I follow for each: what the attack actually does, what I check in SIEM (Monitoring, Log Management), what Web/Endpoint security would show, how I confirm, what I do next. I tag **MITRE** and **NIST** to keep it standard.

---

## SQL Injection (SQLi)

The web-server takes user input and puts it into a SQL query without cleaning it. The database then runs what the attacker wrote.

**What the attacker can do:** read tables, change data, delete data. In some setups, reach the OS via DB features.

**Where I look**
- **Monitoring:** WAF/IDS alerts for things like `UNION SELECT`, `OR 1=1`, `sleep(`.
- **Log Management:** very long query strings; quotes and comment markers (`'`, `--`, `/* */`); encoded versions `%27` `%2D%2D`. Spikes of 500/403 errors.

**Quick confirm:** payloads like `' OR 1=1--` or `' UNION SELECT …`; scanner user-agent `sqlmap`.

**What I do**
- Block the pattern/IP at WAF/CDN (temporary).
- Fix code: use parameterized queries and input validation.
- Re-test with the same payloads.

**Tags:** MITRE **T1190**; if OS commands run then **T1059**. NIST **Detect → Respond → Recover**.

---

## Cross-Site Scripting (XSS)

**Plain English:** attacker’s JavaScript gets echoed by our site and runs in the victim’s browser. The server isn’t owned, but it delivers the script.

**What the attacker can do:** steal sessions, redirect, click stuff as the user.

**Where I look**
- **Monitoring:** WAF/CSP hits for `<script>`, `onerror=`, `javascript:`.
- **Log Management:** params containing `<` `>` or `%3C %3E`, search fields (`q=`, `s=`) with tags.

**Quick confirm:** payloads like `"><img src=x onerror=alert(1)>`.

**What I do**
- Fix templates: output-encode user data.
- Enforce CSP; add WAF rule for common XSS strings.
- Re-test.

**Tags:** MITRE **T1189** (client side), app probing under **T1190**. NIST **Detect → Respond → Recover**.

---

## Command Injection

The attacker sends user input to the OS shell. The server executes commands the attacker chose.

**What the attacker can do:** run commands, download tools, open a reverse shell.

**Where I look**
- **Monitoring:** WAF/IDS on shell operators `;`, `&&`, `|`, backticks, `$()`.
- **Log Management:** suspicious commands in headers like `User-Agent`; 500 errors when payloads hit.
- **Endpoint:** process tree shows web server spawning `sh/bash/cmd/powershell`; outbound connections.

**Quick confirm:** strings like `;cat /etc/passwd` or `| powershell -enc …`.

**What I do**
- Isolate the server if execution happened; capture processes/files.
- Block operator patterns at WAF.
- Fix code to avoid shell calls or strictly whitelist args.

**Tags:** MITRE **T1190**, **T1059**. NIST **Detect → Respond → Recover**.

---

## IDOR (Insecure Direct Object Reference)

The app doesn’t check authorization. Changing `id=123` to `id=124` shows someone else’s data.

**What the attacker can do:** read or edit other users’ objects.

**Where I look**
- **Monitoring:** unusual number of objects accessed per session.
- **Log Management:** many sequential `id=` values from the same user/IP; same response sizes.

**Quick confirm:** try with a low-priv test account; if 200 OK on other users’ objects, it’s IDOR.

**What I do**
- Add server-side authorization checks (deny by default).
- Stop exposing sensitive IDs in URLs (use opaque IDs).
- Re-test.

**Tags:** MITRE fits **T1190** (logic abuse). NIST **Detect → Respond → Recover**.

---

## LFI / RFI (Local / Remote File Inclusion)

 The attacker uses user input to choose a file.  
- **LFI:** reads a local file like `/etc/passwd`.  
- **RFI:** fetches and includes a remote file, which can execute.

**What the attacker can do:** read sensitive files (LFI). Run remote code (RFI).

**Where I look**
- **Monitoring:** traversal patterns `../`, `%2e%2e%2f`, `php://`; for RFI, `http://` in a file param.
- **Log Management:** requests for `/etc/passwd`, `web.config`; for RFI, see follow-on outbound fetches.
- **Endpoint (RFI):** interpreters launching, dropped scripts.

**Quick confirm:** `file=../../../../etc/passwd%00` or params holding full URLs.

**What I do**
- WAF: block traversal and external protocols in file params.
- Code: normalize path, reject `..`, allowlist filenames.
- Re-test; isolate if code executed.

**Tags:** MITRE **T1190**; data reads **T1005**. NIST **Detect → Respond → Recover**.
