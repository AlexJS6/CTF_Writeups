# This is a writeup of: Hack Park
## Difficulty: Medium

---

Name: Alexander Spiesberger   
Contact: alex.spiesberger@gmail.com  
Date: 3 March 2021

---

![CTF](assets/1.png)

We first start, as always with the scan, nmap, gobuster and nikto:   

![scanning](assets/2.png)

While waiting for the scan to end, we can look a bit around on the website.   
We find a login, a contact form and other stuff.   
Gobuster and Nikto both found a robots.txt file, we go and take a look at it, but nothing too crazy.   
On the task they tell us to brute force the login, I actually don't find any credentials, so I must admit I took the hint.  
So I know now, that the username is admin.  
To brute it, I intercept the request with burp: 

![brutpsuite](assets/3.png)

You can then copy what can be seen in the screenshot.   
We will feed this to hydra:   

![hydra](assets/4.png)

Quick explaination of what is done here:   
- First change is to were the file is (color: red)
- Second change is to the USER (color: green)
- Third change is to PASS (color: blue)
- Last change is the error message (color: yellow)

![error msg](assets/error.png)

With the hydra launched, we get back a password, YAY:

![pass](assets/5.png)

We can now connect to it:

![connection](assets/6.png)

So, we are now connected to a CMS as, normally admin.   
We search a version number, and find one in the about section:  

![version](assets/7.png)

We find on searchsploit a remote code execution for this version.  
We copy it to our folder and see what it does:   

![searchsploit](assets/8.png)



