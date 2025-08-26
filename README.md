<h1>OWASP Juice Shop Security Lab</h1>


<h2>Description</h2>
This project focused on conducting a penetration test against the OWASP Juice Shop vulnerable web application using the TryHackMe training platform. The goal was to identify, exploit, and document common web application vulnerabilities in a controlled lab environment.
We began by setting up the lab environment on TryHackMe and configuring our attack machine (Kali Linux with tools such as Burp Suite, SQLMap, ZAP Proxy, and SecLists). The engagement covered multiple phases of a real-world penetration test, including reconnaissance, exploitation, brute-force attacks, injection flaws, and bypassing access controls.
Key vulnerabilities tested and exploited included:


- SQL Injection to bypass login and enumerate the database schema.
- Brute-force attack against the administrator’s credentials using Burp Suite Intruder with SecLists wordlists.
- Account compromise by exploiting weak security questions and poor password recovery mechanisms.
- Directory traversal and information disclosure via publicly accessible /ftp/ files.
- Poison Null Byte bypass to download restricted files by evading server-side file extension filters.
- XSS vulnerabilities (reflected, stored, and DOM-based) to execute arbitrary JavaScript in users’ browsers.
- Business logic flaws such as accessing another user’s shopping basket and removing 5-star reviews as an administrator.


Through these exercises, we demonstrated how insecure coding practices, weak access controls, and improper input validation create high-risk vulnerabilities that attackers can exploit. Each vulnerability was tested, exploited, and documented with evidence (screenshots, intercepted requests, and server responses).
This project provided hands-on experience in identifying and exploiting vulnerabilities in modern web applications, reinforcing secure coding best practices and improving penetration testing skills in line with OWASP Top 10 risks. <br />


<h2>Languages and Utilities Used</h2>

- SQL → leveraged during SQL Injection attacks to manipulate backend queries.
- JavaScript → targeted in XSS (Cross-Site Scripting) attacks (reflected, stored, DOM-based).
- HTML/CSS → used for understanding forms, input fields, and request structures in the web app.
- HTTP/HTTPS protocols → analyzed in requests and responses for injection points and authentication bypass.
- Burp Suite → for intercepting/modifying HTTP requests, Intruder brute-force, and vulnerability exploitation.
- OWASP ZAP (Zed Attack Proxy) → for scanning, crawling, and identifying vulnerabilities.
- SQLMap → for automating SQL Injection testing.
- SecLists → used as wordlists (e.g., brute-forcing logins, password guessing).
- Kali Linux utilities:
- curl and wget → to fetch files and test endpoints.
- cat, grep, less → to search through wordlists or analyze file contents.
- TryHackMe AttackBox (or Kali VM) → the testing environment.

<h2>Environments Used </h2>

- Kali Linux (Try HackMe AttackBox Virtual Machine)
Served as the main penetration testing environment, preloaded with tools such as Burp Suite, OWASP ZAP, SQLMap, and wordlists (SecLists).
- OWASP Juice Shop Application
The deliberately vulnerable web application used as the target for attacks. Hosted within a controlled lab environment (TryHackMe AttackBox).
- Web Browsers (Firefox / Chromium in Kali)
Used for interacting directly with the Juice Shop interface, testing inputs, and verifying successful exploitation results.


<h2>Step-by-Step Guide:</h2>

<p align="center">
Step 1 – Lab setup

1.	Create/Sign in to TryHackMe then Join OWASP Juice Shop room, use the following link:

https://tryhackme.com/room/owaspjuiceshop

<br/>
<img src="https://i.imgur.com/WmzigLB.png"/>
<br />
<br />
2.	Navigate to Task 1 Open your business and click Start Machine:  

<br/>
<img src="https://i.imgur.com/OJf7wmt.png"/>
<br />
<br />
3.	Once the Target machine is started you will see its IP Address: 

<br/>
<img src="https://i.imgur.com/yQXdIXH.png"/>
<br />
<br />
4.	Click Start AttackBox then select Use AttackBox:  

<br/>
<img src="https://i.imgur.com/eKMJ6MI.png"/>
<br />
<br />
Step 2—Intercept login request. 

1.	Open and start Burp Suite, navigate to Proxy tab and Open Browser (make sure Intercept is off):

<br/>
<img src="https://i.imgur.com/HHdHOry.png"/>

<img src="https://i.imgur.com/rhia7iA.png"/>

<br />
<br />
2.	Copy your Target IP from the TryHackMe page, paste on the Burp’s Open browser:  

<br/>
<img src="https://i.imgur.com/KbUdEqS.png"/>
<br />
<br />
3.	Navigate to Account then click Login: 

<br/>
<img src="https://i.imgur.com/ST1BqmK.png"/>
<br />
<br />
4.	After we navigate to the login page, enter some data into the email and password fields:  

<br/>
<img src="https://i.imgur.com/msIAcJM.png"/>
<br />
<br />
5.	Before clicking Log in, go to Burp and make sure Intercept mode is on. This will allow us to see the data been sent to the server: 

<br/>
<img src="https://i.imgur.com/waTddcP.png"/> 

<img src="https://i.imgur.com/W4PF3Gu.png"/>
<br />
<br />
6.	We will now change the "abc" next to the email to: ' or 1=1-- and forward it to the server: 

<br/>
<img src="https://i.imgur.com/mfEkeK9.png"/>

<img src="https://i.imgur.com/mfEkeK9.png"/>
<br />
<br />
7.	Click Forward until the login completes:  

<br/>
<img src="https://i.imgur.com/iVmlNO6.png"/>
<br />
<br />
8.	Switch back to your browser, you should now be logged in as admin@juice-sh.op: 

<br/>
<img src="https://i.imgur.com/AzSn4m8.png"/>
<br />
<br />
9.	Logout and navigate to the login screen, enter ‘ 1=1-- in the email field, type something in the password field and press Log in, you should see a similar flag:

<br/>
<img src="https://i.imgur.com/kNhqAXb.png"/>
<br />
<br />
10.	Now we will now log into Bender's account. Enter bender@juice-sh.op’-- in the email field, type some password and click Log in, you should see these screens: 

<br/>
<img src="https://i.imgur.com/uqCHLhP.png"/>

<img src="https://i.imgur.com/0CLiQxb.png"/>
<br />
<br />
11.	Capture the login request using Burp, but this time we will put bender@juice-sh.op'-- as the email, and abc as the password then forward it:  

<br/>
<img src="https://i.imgur.com/QATqsfM.png"/>
<br />
<br />
12.	Go back to the Juice Shop web page, now you should be logged in: 

<br/>
<img src="https://i.imgur.com/ll6GXms.png"/>
<br />
<br />
Step 3—Bruteforce the Administrator account’s password.

1.	Open the Burp browser, go to the Login page and enter admin@juice-sh.op as email and enter anything in the password field:

<br/>
<img src="https://i.imgur.com/WAyj9n3.png"/>
<br />
<br />
4.	Go to Burp and turn Intercept On: 

<br/>
<img src="https://i.imgur.com/DVqRF8Y.png"/>
<br />
<br />
5.	Go back to the Login page and click Login, this will capture the request on Burp:	 

<br/>
<img src="https://i.imgur.com/JV0ZkWc.png"/>

<img src="https://i.imgur.com/k3R0lGc.png"/>
<br />
<br />
4.	Right-click the request and Send to Intruder: 

<br/>
<img src="https://i.imgur.com/oMm2e0n.png"/>
<br />
<br />
5.	Go to Intruder and click Clear §: 

<br/>
<img src="https://i.imgur.com/1UmB7mY.png"/>
<br />
<br />
6.	In the request body, replace the password value with §§ (inside the quotes), so it’s exactly:

{"email":"admin@juice-sh.op","password":"§§"}

7.	Click the Payloads tab, for Payload type choose Runtime file and load the the wordlist file best1050.txt which will be located in /usr/share/wordlists/seclists/Passwords/Common-Credentials/best1050.txt, then click Start Attack:

<br/>
<img src="https://i.imgur.com/yvE3pzS.png"/>
<br />
<br />
10.	Failed requests will receive a 401 Unauthorized whereas a successful request will return a 200 code: 

<br/>
<img src="https://i.imgur.com/w8yDA8d.png"/>
<br />
<br />
11.	Click on the request with 200 code, you’ll see the actual password of the email admin@juice.sh.op which is admin123: 

<br/>
<img src="https://i.imgur.com/XlhEoFn.png"/>
<br />
<br />
12.	Reset Jim’s password. If Burp is intercepting, turn intercept off, open the target site, navigate to the Login page and click Forgot Password: 

<br/>
<img src="https://i.imgur.com/5PCJ5Qt.png"/>
<br />
<br />
13.	In Email, enter: jim@juice-sh.op, you’ll see the Security Question “Your eldest siblings middle name?”: 

<br/>
<img src="https://i.imgur.com/LDJbSAK.png"/>
<br />
<br />
14.	Researching about Jim on Google search we found that his brother’s middle name is Samuel: 

<br/>
<img src="https://i.imgur.com/ga9NTMp.png"/></p>
<br />
<br />
15.	Go back to the password’s reset page, answer the Security Question and click Change: 

<br/>
<img src="https://i.imgur.com/eYqVFLX.png"/>
<br />
<br />
16.	Done! You’ve successfully reset Jim’s password: 

<br/>
<img src="https://i.imgur.com/CbcGsZ2.png"/>
<br />
<br />
Step 4—Access the Confidential Document.

1.	Navigate to the About Us page, and hover over the "Check out our terms of use":

<br/>
<img src="https://i.imgur.com/nDLasmd.png"/>

<img src="https://i.imgur.com/fDOrGcj.png"/>
<br />
<br />
2.	The page contains a link to http://10.201.17.47/ftp/legal.md. Visiting the /ftp/ directory reveals that it is publicly accessible, exposing its contents to anyone: 

<br/>
<img src="https://i.imgur.com/DOeS0Kn.png"/>
<br />
<br />
3.	Click the link or manually change the address bar to http://10.201.17.47/ftp/ (remove legal.md). You should see a directory listing with files such as acquisitions.md, legal.md, etc. 

<br/>
<img src="https://i.imgur.com/IEyl7xw.png"/>
<br />
<br />
4.	Click acquisitions.md to open it, then save it: 

<br/>
<img src="https://i.imgur.com/Ab8Nlt7.png"/>
<img src="https://i.imgur.com/07U4S5a.png"/>
<img src="https://i.imgur.com/O5kUCiS.png"/>
<br />
<br />
5.	Navigate to the home page to receive the flag: 

<br/>
<img src="https://i.imgur.com/6puCGZc.png"/>
<br />
<br />
6.	Log into MC SafeSearch’s account. Start by watching the following video:

https://www.youtube.com/watch?v=v59CX2DiX0Y&t=159s 

<br/>
<img src="https://i.imgur.com/nyPx8Q4.png"/>
<br />
<br />
7.	In the video, it’s revealed that his password is “Mr. Noodles.” He mentions replacing some vowels with zeros, specifically changing the letter “o” to “0.” This means the actual password for the mc.safesearch@juice-sh.op account is Mr. N00dles.

8.	Download the Backup file. Navigate back to http://10.201.17.47/ftp/,  try to access package.json.bak, you’ll see the request is blocked with a 403 error stating that downloads are restricted to .md and .pdf files only:

<br/>
<img src="https://i.imgur.com/TAylxDl.png"/>
<img src="https://i.imgur.com/2TyIS1v.png"/>
<br />
<br />
11.	We can bypass this restriction using a technique known as a “Poison Null Byte” represented as %00. Since we’re downloading the file via a URL, the percent sign must be URL-encoded, turning %00 into %2500. By appending %2500 followed by .md to the filename, we can trick the server into allowing the download and bypass the 403 error: 

<br/>
<img src="https://i.imgur.com/zJowzHJ.png"/>
<br />
<br />
12.	Press Enter and it will download instantly:

<br/>
<img src="https://i.imgur.com/56nYlLg.png"/>
Note: A Poison Null Byte functions as a NULL terminator. When placed within a string at a certain position, it instructs the server to stop processing anything that follows, effectively truncating the string and ignoring the remaining characters.
<br />
<br />
13.	Get the YAML file via a Poison-Null-Byte bypass. In your browser, open the public listing: 

http://<TARGET-IP>/ftp/ in this case http://10.201.62.42/ftp/

<br/>
<img src="https://i.imgur.com/OWSa3LU.png"/>
<br />
<br />
12.	Note the file suspicious_errors.yml (it’s blocked by the “only .md/.pdf” rule): 

<br/>
<img src="https://i.imgur.com/sPp1Hpn.png"/>
<br />
<br />
13.	Open Burp, go to Proxy and turn Intercept On. Then go back to your browser and refresh the page, this will capture a request. Right-click the request in and Send to Repeater (this just opens a tab you can edit), or click Repeater and create a new request: 

<br/>
<img src="https://i.imgur.com/afn7YIA.png"/>
<br />
<br />
14.	Navigate to the Repeater, the on the Request change the first line to GET /ftp/suspicious_errors.yml%2500.md HTTP/1.11 and click Send: 

<br/>
<img src="https://i.imgur.com/ET1uI0y.png"/>
<br />
<br />
15.	You’re getting a 200 OK response with Content-Type: text/yaml and readable YAML content (e.g., a title: field), which proves the poison null byte trick worked. The request to suspicious_errors.yml%2500.md bypassed the extension filter and served the .yml file: 

<br/>
<img src="https://i.imgur.com/N31HaMZ.png"/>
<br />
<br />
16.	In Repeater, right-click the response, save it and name it suspicious_errors.yml and cite this as evidence in your report: 

<br/>
<img src="https://i.imgur.com/iRDVm4k.png"/>
<img src="https://i.imgur.com/5ugMt0T.png"/>

We browsed the site’s /ftp/ directory and saw suspicious_errors.yml, which the app blocks with an “only .md/.pdf” rule. Using Burp, we intercepted that request, sent it to Repeater, and changed the first line to GET /ftp/suspicious_errors.yml%2500.md HTTP/1.1. The %2500 decodes to a NULL byte on the server, so the file-type check sees the allowed .md while the file open stops at .yml, returning the YAML. After the 200 OK with Content-Type: text/yaml, we saved the response as suspicious_errors.yml. This step demonstrates a poison null byte download bypass and collects evidence of the flaw for the report (showing that the extension filter can be trivially evaded).
<br />
<br />
17.	Dump schema via UNION SQLi on the search API. Open Burp, turn Intercept On then go back to the target site and search for banana in the search bar (to generate a request): 

<br/>
<img src="https://i.imgur.com/hDvMrus.png"/>
<br />
<br />
18. In Burp, right-click that request to /rest/products/search?q= and send to Repeater: 

<br/>
<img src="https://i.imgur.com/xiuLmDo.png"/>
<br />
<br />
19.	In Repeater, replace the first line with:

GET /rest/products/search?q=banana'))UNION%20SELECT%20sql,2,3,4,5,6,7,8,9%20FROM%20sqlite_master-- HTTP/1.1

Then click Send: 

<br/>
<img src="https://i.imgur.com/kJk5mNX.png"/>
<br />
<br />
20.	Search the JSON for the DDL. In your Repeater response use the search box at the bottom (or Ctrl-F) for CREATE TABLE then save the evidence: 

<br/>
<img src="https://i.imgur.com/SXKHM0z.png"/>
Note: The screenshot shows the UNION-based SQL injection worked on the product search API. Instead of only returning normal product JSON, one of the objects now has an "id" field that contains a long string starting with CREATE TABLE. That string is the DDL (table-creation statement) read from SQLite’s internal catalog (sqlite_master) for the table Addresses—it lists all of that table’s columns (UserId, id PK AUTOINCREMENT, fullName, mobileNum, zipCode, streetAddress, city, state, country, createdAt, updatedAt, etc.) and constraints. Seeing raw CREATE TABLE … text inside the API response means your injected query (e.g., ... UNION SELECT sql, ... FROM sqlite_master ... --) was executed and its result was merged into the app’s results, proving the endpoint is vulnerable and allowing you to enumerate the database schema (and, with follow-ups, dump data).
<br />
<br />
Step 5—Access the administration page.

1.	Login into the administrator’s account.

2.	Open the Debugger on Chromium. Click the three dots (⋮) at the top-right, hover over More tools and click Developer tools. Inside the Developer tools panel click on Sources, this is the Debugger:

<br/>
<img src="https://i.imgur.com/KgCe0pD.png"/>
<img src="https://i.imgur.com/R0kAp7z.png"/>
<br />
<br />
5.	Enter http://10.201.75.142/main-es2015.js on Chromium bar and press Enter: 

<br/>
<img src="https://i.imgur.com/bdYSvfN.png"/>
<br />
<br />
6.	Click the { } (pretty-print) button at the bottom to make the JavaScript file easier to read. Next, use the search function and look for the keyword “admin”. You’ll see several results containing that word, but the important one to focus on is path: 'administration': 

<br/>
<img src="https://i.imgur.com/ZC2YlM2.png"/>
<img src="https://i.imgur.com/J8BqXbX.png"/>
Note: This indicates the existence of a page at /#/administration, which is revealed alongside other paths like about. A more secure design would ensure that only the necessary sections of the application are loaded for each user role, preventing sensitive areas—such as an admin page—from being exposed or discoverable by unauthorized users.
<br />
<br />
7.	View another user’s shopping basket.  Start by login to the Adminstrator’s account:

<br/>
<img src="https://i.imgur.com/nw5Nqex.png"/>
<br />
<br />
8.	On Burp turn Intercept On, then click Your Basket at the administrator’s account page:

<br/>
<img src="https://i.imgur.com/FwHaCic.png"/>
<br />
<br />
9.	Forward each request until you see: GET /rest/basket/1 HTTP/1.1

<br/>
<img src="https://i.imgur.com/FljDxNf.png"/>
<br />
<br />
10.	Now, we are going to change the number 1 after /basket/ to 2 and forward it:

<br/>
<img src="https://i.imgur.com/9A7H2rQ.png"/>
<br />
<br />
11.	It will now show you the basket of UserID 2. You can do this for other UserIDs as well, provided that they have one:

<br/>
<img src="https://i.imgur.com/dxeWJyg.png"/>
<br />
<br />
12.	Remove all 5-star reviews. Navigate to the  http://10.201.89.68/#/administration page again and click the bin icon next to the review with 5 stars:

<br/>
<img src="https://i.imgur.com/i04NjFL.png"/>
<br />
<br />
Step 6—Perform a DOM XSS.

1.	Login to the administrator’s account.

2.	Use an <iframe> payload that executes JavaScript when rendered:

<iframe src="javascript:alert('xss')"></iframe>

<br/>
<img src="https://i.imgur.com/kQZSJJ8.png"/>

Pasting this into the site’s search field causes a pop-up, proving code execution. The trick relies on a common HTML element (iframe), but many others can achieve the same effect (e.g., img/svg with event handlers, or a script tag). This variant of XSS is often called Cross-Frame Scripting (XFS) and is a quick way to detect reflected XSS.

<img src="https://i.imgur.com/bovNTtN.png"/>

In this step you proved an XSS flaw in the site’s search feature by submitting an HTML payload <iframe src="javascript:alert('xss')"></iframe> into the search box. The application reflected that value back into the page without sanitizing or encoding it, so the browser parsed it as HTML and executed the javascript: URL, popping an alert. Because the vector uses an <iframe>, it’s often called Cross-Frame Scripting (XFS); functionally it demonstrates a DOM/reflected XSS path. The result confirms arbitrary script execution via user input; proper defenses are output encoding, strict input validation, and a CSP that blocks javascript: URLs.
<br />
<br />
3.	Perform a persistent XSS. Click on the trigram ☰ at the top-left side of the header, then select Privacy & Security and click Last Login IP:

<br/>
<img src="https://i.imgur.com/efjhVFQ.png"/>

<img src="https://i.imgur.com/c6NcuD9.png"/>
Note: The Last Login IP box just shows whatever address the site thinks you logged in from. In this lab the app sits behind a proxy, so it sees a private network address (like 10.201.x.x) instead of your real public IP. It also trusts proxy headers (e.g., X-Forwarded-For / True-Client-IP) and prints that value straight to the page. That’s why you see an internal IP—and why spoofing those headers makes the page show any value you choose.
<br />
<br />
4.	On Burp turn the intercept on, then logout the administrator’s account. It will catch the logout request. Select Raw view, insert a new header line under User-Agent. Then forward the request to the server:

	True-Client-IP: <iframe src="javascript:alert('xss')"></iframe>
 
<br/>
<img src="https://i.imgur.com/BMbvoNN.png"/>
<br />
<br />
5.	When signing back into the admin account and navigating to the Last Login IP page again, you will see the XSS alert:

<br/>
<img src="https://i.imgur.com/YuYXJvF.png"/>

Why send this header?
True-Client-IP (like X-Forwarded-For) tells the server/proxy what the client’s IP is. Because the application reflects this value without validating or encoding it, we can inject HTML/JavaScript into the header and trigger XSS.

<br />
<br />
6.	Perform a reflected XSS. Login into the administrator’s account and navigate to the 'Order History' page:

<br/>
<img src="https://i.imgur.com/Pxrqx6E.png"/>
<br />
<br />
7.	Click the truck icon to open the order tracking page. In the resulting URL, you’ll notice an id parameter that corresponds to that order:

<br/>
<img src="https://i.imgur.com/P4955Ow.png"/>
<img src="https://i.imgur.com/VzPQdrr.png"/>
<br />
<br />
8.	We will use the iframe XSS, <iframe src="javascript:alert(`xss`)">, in the place of the 5267-ac4355d2e17ecd4a. After submitting the URL, refresh the page and you will then get an alert saying XSS:

<br/>
<img src="https://i.imgur.com/ePBssC8.png"/>
Note: The server maps each tracking code to a record in its database. Because the application blindly accepts and reflects the idvalue without sanitizing or encoding it, we can inject HTML/JavaScript through the id parameter and trigger an XSS.

Conclusion:

This project provided practical, hands-on experience in identifying and exploiting vulnerabilities commonly found in modern web applications, aligning closely with the OWASP Top 10 risks. By working within the controlled environment of the OWASP Juice Shop on TryHackMe, we applied real penetration testing methodologies—from reconnaissance and injection attacks to brute-force exploitation, access control bypasses, and cross-site scripting (XSS).

The engagement demonstrated how insecure coding practices, weak authentication mechanisms, and insufficient input validation can expose sensitive data and administrative functionality. Key findings included successful SQL injection leading to account compromise, brute-force cracking of administrator credentials, exploitation of poor password recovery mechanisms, directory traversal, and XSS attacks (reflected, stored, and DOM-based).

Through these exercises, the lab reinforced the importance of secure coding practices, strong authentication, proper access control, and strict input/output validation in safeguarding applications. It also strengthened penetration testing skills by simulating real-world attack vectors, analyzing evidence, and applying exploitation tools such as Burp Suite, OWASP ZAP, and SQLMap.

Overall, the project not only enhanced technical proficiency in offensive security but also emphasized the critical need for proactive defense and security awareness during the software development lifecycle.

<br />
<br />
</p>
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
