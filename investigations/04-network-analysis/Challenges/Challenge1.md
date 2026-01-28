![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/image1.png)

I’ve been given a PCAP file for analysis. The plan is to treat this like a CTF style challenge, go through the capture step by step, pull out suspicious traffic, and piece together what the attacker was trying to do.

![A blue and white rectangle with text AI-generated content may be
incorrect.](media1/media/image2.png)

I started by filtering out the SMTP traffic with the smtp filter. This gives me only the email-related packets to work with, which makes it easier to focus on the traffic that actually matters for the question.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/image3.png)

Now I’ll go through each SMTP packet quickly, checking the headers and payloads in fast forward to see if anything stands out like sender and recipient info, suspicious subjects, or attachments.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/image4.png)

Looks like we’ve landed on the actual email. The name is kind of cliché, but this should be the one worth digging into further.

![A blue and black rectangular object AI-generated content may be
incorrect.](media1/media/image5.png)

To answer this part I need to focus on the packet where the authentication actually happens. That’s where the login details are exchanged, and it should give a clearer picture of how the attacker is getting access.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/image6.png)

From this point I’ll right click on the packet and follow the TCP stream. That way I can see the entire conversation in order instead of digging through single packets one by one.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/image7.png)

I can see the credentials are hex encoded in the TCP stream. Once I decoded it, the password showed up ,same one I already noted in the answer above.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/image8.png)

To answer this part I need to look directly at the packet where the email transaction actually took place. That packet will show the details of the mail being sent, instead of just the authentication or setup traffic.

![A screenshot of a computer code AI-generated content may be
incorrect.](media1/media/image9.png)

When I followed the TCP stream, I found the actual contents of the email. This view lays out the full conversation in plain text, which makes it easier to see the sender, recipient, subject, and the body of the message.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/image10.png)

To analyze the content of the .docx, I dumped the entire encoded section into CyberChef. That gave me a readable output and meaningful results instead of staring at raw encoding inside the TCP stream.

From a MITRE ATT&CK perspective, this falls under T1566 (Phishing) since the attacker is delivering a malicious attachment via email. The decoding step confirms the payload delivery mechanism. If the macro inside the .docx launches external connections, it would also map to T1105 (Ingress Tool Transfer).

According to NIST 800-61 (Incident Handling Guide), this activity fits into the detection and analysis phase. The PCAP and CyberChef decoding provide the evidence needed to confirm that the document is malicious. The next step in a NIST workflow would be containment and eradication if this were a live incident.
