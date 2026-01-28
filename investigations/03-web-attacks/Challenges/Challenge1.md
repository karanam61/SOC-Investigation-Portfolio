Web attacks Challenge 1 :

<img src="./media2/media/image1.png"
style="width:6.5in;height:2.43056in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

We’ve got the source and destination IPs. Next step is log management to see if there were any additional exchanges between these two.  

The plan is simple: pull up the SIEM logs and check for repeated traffic, odd request patterns, or weird ports. If the IPs are chatting more than they should, that’s enough to flag.  

MITRE lines up here under **Command and Control (C2)** since attackers love keeping comms open between victim and their infrastructure.

<img src="./media2/media/image2.png"
style="width:6.5in;height:3.50347in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

The traffic is going over **port 443**, so it’s using HTTPS. The status code returned is **500**, which means the app or code crashed. That usually points to the attack not being successful.

But we can’t just assume. We need to cross-check in **Endpoint Detection** and look at the processes to see if anything actually ran or got returned. Response size here is **0**, which adds to the case that nothing meaningful came back.

Let us look at the request now :

<https://172.16.17.13/?file=../../../../etc/passwd>

This looks suspicious, a random cloud provider is making a request for the `/etc/passwd` file. That path being targeted is a red flag, since attackers often go after it to enumerate users on the system. The logs show the request going up the hierarchy, which makes it clear this isn’t normal behavior.

<img src="./media2/media/image3.png"
style="width:6.5in;height:3.04653in"
alt="A screenshot of a computer error AI-generated content may be incorrect." />

he source IP address resolves back to **Tencent Cloud**, which is obviously a web-hosted provider. That makes the activity even more suspicious, since attackers often spin up short-lived servers on cloud platforms for scanning, exploitation, and staging attacks.

<img src="./media2/media/image4.png"
style="width:6.5in;height:3.41389in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

Only **1/94 security vendors** flagged the source IP as malicious, which by itself doesn’t hold much weight. The destination in this case is a **LetsDefend.io company webserver**, and the traffic is clearly coming from the internet.

**Last Login:** Feb 19, 2022, 01:01 PM  

When checking the processes and web browser history on the endpoint, nothing suspicious showed up. This suggests that even though the traffic looked questionable, there’s no solid evidence of compromise on the host.

<img src="./media2/media/image5.png"
style="width:6.5in;height:3.63333in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

<img src="./media2/media/image6.png"
style="width:6.5in;height:2.90486in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

Since the suspicion was around **Local File Inclusion (LFI)**, the terminal would need to be accessed to move up or down the directory hierarchy. After checking, there’s nothing in the logs or processes that supports the alert.

This makes it clear — it’s not a true positive. It’s a **false positive**, because nothing actually suspicious was found on the endpoint.

<img src="./media2/media/image7.png"
style="width:6.5in;height:2.94514in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

It was a possible LFI attack but wasn’t successful .
