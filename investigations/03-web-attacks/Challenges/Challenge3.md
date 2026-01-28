Challenge – HTTP :

The challenge is analyzing a PCAP file. First step is to load it in a tool like Wireshark and get a quick overview — what protocols are present, how many packets, and the capture timeline.  
This sets the baseline for what’s normal and what’s not.
Check out the source and destination IPs and the ports being used. Common web traffic over 80 or 443 isn’t always safe, so filtering in the SIEM helps confirm if those same IPs show up in historical logs.  
This ties into **NIST Incident Response (IR-4: Incident Handling)** — identify potential malicious communications early.
Look for clear red flags:
- HTTP requests with encoded payloads (could point to LFI or SQLi).  
- Attempts to access sensitive paths like `/etc/passwd`.  
- Unusual DNS queries that could signal tunneling.  

These map to **MITRE ATT&CK T1190 (Exploit Public-Facing Application)** and **T1046 (Network Service Scanning)**, depending on the activity.

Once suspicious traffic is spotted, validate it:
- Cross-check in SIEM for matching logs.  
- Correlate timestamps with endpoint data (EDR) to see if anything executed.  
- Confirm if this is a true positive or just noise.  


<img src="./media4/media/image1.png"
style="width:6.5in;height:3.84931in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

<img src="./media4/media/image2.png"
style="width:6.5in;height:1.95069in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

<img src="./media4/media/image3.png"
style="width:6.5in;height:3.27847in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

<img src="./media4/media/image4.png"
style="width:6.5in;height:1.32153in"
alt="A blue and black stripe AI-generated content may be incorrect." />

<img src="./media4/media/image5.png"
style="width:6.5in;height:1.21875in"
alt="A blue and white rectangle with white text AI-generated content may be incorrect." />

<img src="./media4/media/image6.png"
style="width:6.5in;height:1.39931in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

<img src="./media4/media/image7.png"
style="width:6.5in;height:4.10556in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

<img src="./media4/media/image8.png"
style="width:6.5in;height:1.15417in"
alt="A screen shot of a computer AI-generated content may be incorrect." />

<img src="./media4/media/image9.png"
style="width:6.5in;height:3.26389in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

The string `d2ViYWRtaW46VzNiNERtMW4=` is just **Base64**. Decoding it gives

webadmin:W3b4Dm1n


That’s a username/password combo. If this shows up in logs or traffic, treat it as sensitive. Base64 isn’t encryption — it’s just an encoding. Attackers know how to run `base64 -d` in a second, so hiding creds like this is pointless.

<img src="./media4/media/image10.png"
style="width:6.5in;height:2.58403in"
alt="A screenshot of a computer AI-generated content may be incorrect." />
