Phishing challenge -- 2 :

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image1.png)

We’ve got the SMTP IP and the source address from this alert. First thing, before doing anything else, I’ll just check the sender’s mail server with an MX lookup. This gives a quick idea if the server looks legit or if it’s some random spoofed setup.

From the SOC side, this ties back to what we’d already see in the SIEM under log management. Email security handles the first flag, and if it actually hit a user machine, endpoint security would pick it up for further action.

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image2.png)

The mail server already looks suspicious since it’s blacklisted. That’s a solid indicator this isn’t coming from a clean source.

Next step is to run an SPF lookup to see if the sending IP is even authorized for this domain. If it fails, that just adds more weight to this being spoofed.

From a SOC angle, this is the point where email security logs in the SIEM confirm the sender policy mismatch. If users received this, endpoint security would help us track which systems actually saw the message.

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image3.png)

PF is enabled, but DMARC isn’t. On top of that, there’s no DKIM either. Basically, only one layer of email authentication is in place, which makes the sender a lot less trustworthy.

From the SOC side, this shows up in the email security logs in the SIEM. Monitoring catches the gaps, log management confirms what’s missing, and if this mail hit a user endpoint, EDR would flag it for potential containment.

Mapped to frameworks:

MITRE ATT&CK T1566 (Phishing) – malicious email delivery.

NIST CSF Detect – identifying weak or missing authentication controls.

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image4.png)

Meaning we can't verify the integrity of the email as the digital
signature might be forged .

Then we'll check whether the email is delivered or not

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image5.png)
Since DKIM and DMARC are missing, we can’t fully verify the integrity of the email — the digital signature could be forged.

Next step is to confirm if the email was actually delivered. Delivery status in the log management console tells us whether this hit the user’s inbox or got filtered along the way. If it made it through, that’s where endpoint security comes in to monitor activity on the recipient machine.

From a framework view:

MITRE ATT&CK T1566 (Phishing) – email delivery vector.

NIST CSF Detect → Protect – identifying incomplete signatures, then checking if the control failed (delivery happened).

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image5.png)

Next step is to check if there are any attachments. For that we pivot to the email security console. This gives us a clear view of whether the message carried files along with it or if it was just a URL-based phish.

From a SOC perspective:

Monitoring / Log Management → confirms the block action.

Email Security → attachment inspection.

Endpoint Security → only relevant if the block had failed and a file was delivered.

Mapped to frameworks:

MITRE ATT&CK T1566.001 – Phishing: Attachment (potential vector).

NIST CSF Detect – verifying controls against suspicious attachments.

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image6.png)

We'll use the hash of the file and go to virus total and anyrun to see
anything malicious .

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image7.png)
VirusTotal flags the sample as malicious, but that alone isn’t enough to close the case — vendor hits can be false positives.

Next step is to run it in AnyRun to watch behavior in real time. That way we can catch if there’s a payload download, network callbacks, or any suspicious process activity that didn’t show up in static checks.

SOC workflow tie-in:

Email Security → initial detection.

Log Management → correlation with threat intel feeds.

Endpoint Security → behavioral monitoring if the file actually lands.

Framework mapping:

MITRE ATT&CK T1204 – User Execution (malware execution via attachment or link).

NIST CSF Detect → Respond (confirming malicious behavior through sandboxing).

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image8.png)

![A screenshot of a computer AI-generated content may be
incorrect.](images1/media/image9.png)

AnyRun shows the attachment is a PDF file flagged as malicious with phishing activity detected. On top of that, there’s evidence of something launching automatically once the file is opened.

So the conclusion is clear: the email was a phishing attempt delivered with a malicious PDF. Since the endpoint security controls blocked execution, the threat was contained. We can stop the investigation here and close out the playbook.

Framework tie-in:

MITRE ATT&CK T1566.001 – Phishing: Attachment

MITRE ATT&CK T1204 – User Execution

NIST CSF Respond → Recover – containment at the endpoint and resolution of the incident.
