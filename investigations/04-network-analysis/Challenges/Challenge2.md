Brute force attack .

# I've been given both pcap aswell as an authentication log to analyze a brute force attack

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image1.png)

Up until log entry 122, I can see the user with IP 196.136.60.15 having multiple successful authentication sessions to the root account. After that point, things change, there are repeated authentication failures coming from IP 141.98.10.105. The number of failures and the frequency of the session closures look unusual, like someone is hammering the login.

Even with those repeated failures, I don’t see any obvious suspicious activity in the rest of the network logs. That makes me think this is probably an internal IP generating noise rather than an external attacker who broke in.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image2.png)

In parallel with the authentication activity, I can see in the network logs that IP 192.168.190.137 is making repeated RDP attempts to 51.116.96.181. Since the behavior looks persistent, the next step is to check for the same IP in the authentication logs to see if there’s any overlap or evidence of brute force activity there as well.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image3.png)

I can also see that the RDP attempts from 192.168.190.137 to 51.116.96.181 were eventually successful since packets are being exchanged between them. That means the server that ended up compromised in this case is definitely 51.116.96.181.

![A screen shot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image4.png)

![A blue and white rectangle with white text AI-generated content may be
incorrect.](media1/media/media2/media/image5.png)

To answer this part I’ll filter Wireshark for HTTP requests, since that’s the only way to see what’s happening with a specific server directory. Once filtered, I can check the requests in detail and figure out what exactly is going on.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image6.png)

I can see multiple POST requests hitting index.php, so it looks like the attacker is targeting that directory to authenticate. This makes sense because POST is commonly used to send login credentials. We can confirm this by looking at the full HTTP/1.1 request, which should show the parameters being passed to index.php

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image7.png)

To double-check this, I also used Statistics → HTTP and Statistics → Endpoints in Wireshark. That confirmed the repeated POST activity against index.php, lining up with what I saw earlier in the filtered traffic.

This way you’re not just relying on the filter, you’re backing it up with Wireshark’s built-in statistics to confirm the pattern.

![A blue and white rectangle with white text AI-generated content may be
incorrect.](media1/media/media2/media/image8.png)

To answer this question I need to dig into the HTTP POST requests directly. Looking at the POST payload will show what parameters are being sent to index.php, which should confirm if the attacker is actually trying to authenticate or brute force login credentials.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image9.png)

The POST requests confirm that a series of usernames and passwords are being sent to index.php. Whether each attempt is correct or not can be checked by looking at the HTTP response. A successful login would show a different response code or a change in the server’s reply compared to the failed ones.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image10.png)

For every wrong username and password combination the server throws back an “incorrect” response, which shows up in red in Wireshark. When the credentials are valid, the server instead replies with the correct response, confirming that the login succeeded.

At first I didn’t know how to apply a text-based filter to highlight these responses. I had to get some external help and found the right way to filter for HTTP response content inside Wireshark. That let me separate the failed login attempts from the successful one and confirm how the brute force actually played out.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image11.png)

What I’ve taken away from this exercise is simple. This is how brute force over HTTP actually looks in real traffic. You see the repeated POSTs with different username and password combinations, the incorrect responses for the bad ones, and the single correct response when the attacker finally lands on valid credentials.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image12.png)

When you look at the sequence of POSTs in Wireshark:

The failed ones show the “incorrect” message in the HTTP response.

The last failed attempt is followed by the successful POST, where the server replies differently (often a 200 OK with a redirect, a session cookie, or some content that proves authentication worked). .

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image13.png

![A blue and white text AI-generated content may be
incorrect.](media1/media/media2/media/image14.png)

We’ve got the right username and password combination. The sequence of POST requests confirmed it ,all the earlier ones failed with “incorrect” responses, but this one came back clean from the server. That makes it the valid login and proves the brute force attempt worked.

In a SOC context this would be treated as a confirmed true positive, mapped to MITRE ATT&CK T1110 (Brute Force) and T1078 (Valid Accounts), and moved into containment and response since a real compromise has been shown.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image15.png)

It’s basically the number of attempted logins with different usernames. To see this clearly we need to separate the POST requests by applying the right filter in Wireshark. That way only the POST traffic shows up, and we can go through each login attempt without the rest of the noise.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image16.png)

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image17.png)

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image18.png)

Instead of trying to handle everything inside Wireshark, I exported the HTTP POST traffic out and worked on it with some basic Linux commands. This made it easier to parse through the requests, count the login attempts, and quickly spot the usernames and password combinations being tested without scrolling through the capture manually.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image19.png)

We get 7 unique usernames .

![A blue and white text AI-generated content may be
incorrect.](media1/media/media2/media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image21.png)

To find anything specific inside a request, or details beyond the usual fields shown, I used a second filter in Wireshark. This lets me search directly for the string or value I’m after instead of digging through every field manually. With the right display filter applied, I can isolate things like parameters, usernames, or response text and confirm if they show up in the traffic. .

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image22.png)

![A blue and white screen AI-generated content may be
incorrect.](media1/media/media2/media/image23.png)

Alright, let’s continue with the next question .

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image24.png)

![A blue and white rectangle with white text AI-generated content may be
incorrect.](media1/media/media2/media/image25.png)

![A blue and white text AI-generated content may be
incorrect.](media1/media/media2/media/image26.png)

Since the question asked for the latest activity, I just scrolled through the capture to the end and found the answer there. That way I could confirm the most recent event instead of only looking at the earlier packets.

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image28.png)

![A blue background with white text AI-generated content may be
incorrect.](media1/media/media2/media/image29.png)

![A blue line in a dark background AI-generated content may be
incorrect.](media1/media/media2/media/image30.png)

![A screenshot of a computer AI-generated content may be
incorrect.](media1/media/media2/media/image31.png)

This traffic is a brute force attempt. The repeated POST requests with different usernames and passwords confirm it. According to MITRE ATT&CK, this falls under T1110 (Brute Force). Once a valid login is found and abused, it also matches T1078 (Valid Accounts).

From a NIST 800-61 incident handling perspective, this sits in the detection and analysis phase. The evidence is clear enough to mark the event as a confirmed incident. The next steps, per NIST, would be to move into containment and eradication by blocking the malicious IP, cutting off further login attempts, and resetting or monitoring the compromised account.

![A blue rectangular object with a black stripe AI-generated content may
be incorrect.](media1/media/media2/media/image32.png)
