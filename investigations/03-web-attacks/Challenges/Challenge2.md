Web attacks – Challenge 2 :

In this challenge I was given an access log to analyze .
question was

<img src="./media3/media/image1.png"
style="width:6.5in;height:1.25139in"
alt="A blue and white rectangle with white text AI-generated content may be incorrect." />

<img src="./media3/media/image2.png"
style="width:6.5in;height:3.50139in"
alt="A screenshot of a computer code AI-generated content may be incorrect." />

I found this suspicious as someone is trying to change domain name extensions like **.show, .jave, .dgps**. At first glance this could look like probing or manipulation of DNS-related requests.  

But the response code returned is **404**, meaning nothing actually resolved or responded. So for now there’s nothing to worry about — no indication that the attempt was successful.

 The question asked for the automated tool used for reconnaissance. In the logs I can see **Nikto 2.1.6** being used. I double-checked this online and confirmed that Nikto is a well-known web server scanner.  

It’s an open source tool that runs automated checks for outdated software, common vulnerabilities, and misconfigurations on web servers. Seeing this in the logs is a clear indicator of reconnaissance activity.
<img src="./media3/media/image3.png"
style="width:6.5in;height:2.79653in"
alt="A screenshot of a computer error AI-generated content may be incorrect." />


So it’s basically a **web vulnerability scanner** in this case. Using **Nikto**, the attacker tried different file extensions to see if any would slip past and expose a weakness.  

The idea here is simple — if the application doesn’t have clean and secure code, these odd extensions might hit a jackpot and reveal hidden files or misconfigurations. This makes it clear that the activity was part of automated reconnaissance.

<img src="./media3/media/image4.png"
style="width:6.5in;height:3.96458in"
alt="A screenshot of a computer AI-generated content may be incorrect." />

<img src="./media3/media/image5.png"
style="width:6.5in;height:2.06597in"
alt="A screen shot of a computer code AI-generated content may be incorrect." />

After reconnaissance, the attacker starts testing web directories one by one. This is basically the first active attack attempt — a **directory brute force**.  

You can clearly see him trying out different directory names, even testing numbers in different formats, just to check if any of them return a valid response. This is a common move attackers use to discover hidden directories or poorly secured paths on the server.

<img src="./media3/media/image6.png"
style="width:6.5in;height:2.62778in"
alt="A screenshot of a computer screen AI-generated content may be incorrect." />

I see some encoding here that points towards a **directory traversal attack**. This looks like the second attack attempt after the brute-forcing.  

So in this challenge we’re not just dealing with one technique — there were multiple attacks happening at once. With the logs reviewed and the patterns confirmed, we’ve solved it.
