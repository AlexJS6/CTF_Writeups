# RootMe

## This is a simple CTF

---

Name: Alexander Spiesberger   
Date: 03/02/2021   
contact: alex.spiesberger@gmail.com   

----

![CTF](assets/ctf.png)

We start by launching nmap and gobuster 

![nmap](assets/1.png)

![gobuster](assets/2.png)

We then go on the web page but nothing:

![RootMe](assets/3.png)  

BUUUT, gobuster find files, (uploads and panel)

![gobuster result](assets/4.png)

With nmap and gobuster we now can respond to all the questions in task 1 and 2.

We now go to panel and find a page were we can upload files.

![uploads](assets/5.png)

We try a reverse shell, I tried the php-reverse-shell that comes with kali linux in: /usr/share/webshells/php  
But when we upload it, it says that php isn't accepted.  

![denied](assets/denied.png)

What we can try is to use another extension, for example: ".phtml"   

And this is a success!

![success](assets/6.png)

We now launch it on the other folder we found: uploads, and yay! we have a our reverse shell!

![reverse shell](assets/7.png)

We now search for the user.txt:

![user.txt](assets/8.png)

We can check the SUIDs with: 

![SUID Search](assets/9.png)

We find an interesting binary:  

![SUID](assets/10.png)

We can find on GTFOBins an escalation method: https://gtfobins.github.io/gtfobins/python/   

So we launch the command:

![command](assets/11.png)

Yay! We are root! We can now search for root.txt

![root.txt](assets/12.png)

Hope you enjoyed the writeup of this CTF.  
contact: alex.spiesberger@gmail.com

