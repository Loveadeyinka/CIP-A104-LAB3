Lab 3 – File Inclusion and SQL Injection Security Assessment

What this is
This repo contains my proof and write-up for Lab 3 in the CIP-A104 Offensive Security module. It is for the ICDFA Ethical Hacker Internship cohort.

For this lab, we studied two usual web issues. They are Local File Inclusion and Remote File Inclusion. We also covered SQL Injection. The lab used two practice apps that are meant to be vulnerable. They are DVWA and OWASP Mutillidae II.

Scope and disclaimer
I ran all checks in a locked Kali Linux VM. I tested only DVWA and Mutillidae. Both were started in local Docker setups. They ran on `127.0.0.1`.

None of this targeted a real public site. Nothing was done to any third party system. This is for training only. Do not copy it and point it at software you do not own. Do not use it where you lack written permission.

What I actually did

File Inclusion
- I first watched how each app's file page works in a normal case. This was my baseline.
- I then placed a small marker file inside each app's container. After that, I used the page parameter to load that file into the output. This showed the app will read a path when you give it one. That is LFI.
- Next, I tried a file name that does not exist. I did it in both apps to see what happens when the file is gone. I also tested a `../` path shape. This helped me see whether the app changes or cleans up the path.
- I ran the same checks again after setting the apps to their highest security mode. I noted what changed with the stricter setting.

SQL Injection

- Sent a plain, valid request to DVWA using the `id` parameter.  
  Also sent a normal request to Mutillidae’s login form, just to see what a clean query looked like.
- Then I tested boolean style inputs.  
  Examples were payloads like `' AND '1'='1` and `' AND '1'='2`.  
  The goal was simple: find out if the app was mixing user text into SQL statements.
- On Mutillidae, I used an `OR` style input tied to a real account value.  
  Instead of one matching user, the page showed many accounts.  
  That showed the input changed the query logic, not only the returned data.
- I reran the same checks at higher security settings on both apps.  
  This was to sort out what was truly fixed, versus what just became harder to trigger.

Tools used
- Kali Linux VM  
- DVWA and OWASP Mutillidae II in Docker  
- Burp Suite or OWASP ZAP for proxying and viewing requests  
- Manual request editing in the proxy tool  
- `curl`, `docker exec`, `sha256sum` for marker setup and verification

What I found
At low security, both apps allowed user input to affect file paths and SQL behavior.  
There was no real filtering.
When security was turned up on DVWA, it stopped the issue in the intended way.  
The app used server side allow lists and parameterized queries.
On Mutillidae, higher settings also helped most of the same problems.  
But its SQL Injection protection came down to a check in the browser side input length.  
It was not a solid server side control.  
So this is weaker than a full fix, and it should be called out as its own result.

Why I chose this method
I kept every test safe.  
Each one used a small marker string or the app's own built in test accounts.  
No real data ever went in.  
And I only used the smallest amount needed to show the result.

That was on purpose.  
It was not due to some missing tool or a hard limit.  
This is how testing like this should be done.  
Even when the work is on a real engagement.
