Phishing Alert 1 :

![A screenshot of a computer error AI-generated content may be
incorrect.](images/media/image1.png)

## Alert Metadata

We collected the following information from the investigation channel (screenshot evidence attached):

- **Source**: `172.16.17.49` (internal workstation – Let’sDefend employee)  
- **Destination**: `91.189.114.8` (external Internet address)  
- **User Agent**:  
  `Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36`

### Framework Mapping

- **MITRE ATT&CK**  
  - **T1566 – Phishing**: the likely initial access vector that led to the employee interacting with a malicious link.  
  - **T1071 – Application Layer Protocol**: suspicious outbound connection over HTTP(S) using a browser-like User Agent.  

- **NIST Cybersecurity Framework (CSF)**  
  - **Detect**: abnormal outbound traffic identified through monitoring.  
  - **Respond**: investigation into source/destination and correlation with user activity.  
  - **Recover**: containment of the workstation and potential remediation steps (credential reset, system reimage).  
Log Management :

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image2.png)

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image3.png).

## Log Management

In the log management console, we found **two logs** tied to the destination IP address.  
- One entry came from the **firewall log**.  
- The other was a **proxy log**.  
Both point to the same URL, confirming that the connection attempt was logged by multiple controls.

---

## URL Analysis

Since this isn’t a typical email-phishing case (no headers to parse), our focus shifts to the **URL itself**.  
We analyze it through third-party platforms:  
- **VirusTotal** – to check domain/IP reputation and historical detections.  
- **AnyRun** – dynamic sandbox detonation to watch behavior.  
- **URLScan.io** – for redirection chains, SSL certs, and hosting details.  

From here the investigation continues with standard checks and correlation against threat intel results.
Url -
<http://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io>

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image4.png)

10 out of 97 vendors flag this URL as malicious, but blindly trusting that isn’t smart. We need to dig deeper with other platforms to actually confirm what’s going on.

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image5.png)

At this point we can flag the URL as suspicious. The main IP is located in the Russian Federation and it’s reaching out to six different IPs across two countries. That alone is shady enough to warrant deeper review.

The domain is also suspicious because it pretends to be part of the Akismet WordPress plugin (`/wp-content/plugins/akismet/`), but it’s actually hosted on a random `.ru` domain with zero connection to the real Akismet service. The URL is calling a PHP script (`index.php`) and passing an email parameter, which isn’t normal for a legit plugin path. In a proper setup, Akismet only processes emails on trusted WordPress installs, not through some external Russian domain.

Anyrun goes a step further and shows that the URL tries to download a suspicious payload for execution.

With all this information combined, I concluded that this is a phishing URL.

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image6.png)

Next part is EDR 

![A screenshot of a computer screen AI-generated content may be
incorrect.](images/media/image7.png)

Clearly, we can see the url was accessed by Emily and I've already
listed out the other things . One important factor here is that the
request is not blocked but was allowed . So the url is accessed and we
need to contain the end point .

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image8.png)

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image9.png)

Lets check if the analysis is right or wrong .

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image10.png)

So the analysis is right .
