Phishing challenge :

## Monitoring

We received a PayPal `.eml` file (opened in Thunderbird). The initial alert came through the SIEM’s **email security module**, which flagged it for review.  

---

## Email Security

### Authentication Checks
- **SPF** → enabled.  
- **DKIM** → not present.  
- **DMARC** → not present.  

This is not unusual, but the lack of DKIM/DMARC reduces sender integrity.  

### SMTP Validation
- SMTP relay is from Google’s MX servers.  
- Nothing stands out here — headers align with expected Google delivery.  

---

## Log Management

No anomalies in the SMTP or mail transfer logs. The source server is valid and shows clean relay paths. However, further analysis is required because missing DKIM/DMARC leaves room for spoofing.  

---

## Endpoint Security

At this stage, there is no sign the email was opened on a host, so no endpoint activity is tied to this sample yet. Monitoring continues to ensure no attachments or links were executed.  

---

## MITRE ATT&CK Mapping
- **T1566 – Phishing**: suspicious PayPal-themed email.  
- **T1585 – Establish Accounts**: possible abuse of legitimate mail infrastructure (Google MX).  

---

## NIST CSF Mapping
- **Identify**: suspicious email reported and collected.  
- **Detect**: header analysis shows missing DKIM/DMARC.  
- **Protect**: SPF validation helps confirm legitimate senders but incomplete configuration noted.  

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image1.png)

![A screenshot of a computer code AI-generated content may be
incorrect.](images/media/image2.png)

![A computer screen shot of a computer code AI-generated content may be
incorrect.](images/media/image3.png)

## Monitoring

The first suspicious indicator comes from the **email security alert** highlighting a mismatch between the `Return-Path` and the `From` address.  

- **Return-Path** → `bounce@rjttznyzjjzydnillquh.designclub.uk.com`  
- **From** → `P.A.Y.P.A.L? <IHKH0MFEWW@kodehexa.net>`  

This mismatch is a classic sign of redirection/spoofing.  

---

## Email Security

- WHOIS lookup confirms no relationship between the return-path domain (`designclub.uk.com`) and the supposed PayPal sender.  
- Case-sensitive and unusual characters in the `From` field (`P.A.Y.P.A.L?`) increase suspicion.  
- These signs suggest the message is designed to redirect the victim rather than originate from a trusted source.  

---

## Log Management

SMTP log correlation shows the mail passed through valid Google MX servers, but the header mismatch confirms tampering at the domain level rather than transport.  

---

## URL Analysis (Email Security → Deep Dive)

The email contains the following URL:  

https://storage.googleapis.com/hqyoqzatqthj/aemmfcylvxeo.html#QORHNZC44FT4.QORHNZC44FT4

yaml
Copy code

Observations:  
- Hosted on Google Cloud Storage → attacker leveraging legitimate infrastructure.  
- Randomized path and hash values → gibberish, no sign of brand alignment.  
- VirusTotal confirms multiple detections of this URL as phishing.  

---

## Endpoint Security

At this point, no evidence that the recipient clicked or executed content locally. If accessed, endpoint telemetry would need to be checked for browser activity and follow-on connections.  

---

## MITRE ATT&CK Mapping
- **T1566 – Phishing**: malicious email with spoofed sender and embedded link.  
- **T1071 – Application Layer Protocol**: use of HTTPS to deliver phishing content.  
- **T1584 – Compromise Infrastructure**: abuse of Google Cloud Storage to host phishing payloads.  

---

## NIST CSF Mapping
- **Detect**: mismatched headers and suspicious URL flagged by monitoring.  
- **Respond**: WHOIS + VirusTotal confirm phishing classification.  
- **Recover**: block listed URL in email and web gateway controls. 
![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image4.png)

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image5.png)

Here you go , this proves that it is a malicious phishing email .

Questions :

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image6.png)

![A screenshot of a computer AI-generated content may be
incorrect.](images/media/image7.png)
