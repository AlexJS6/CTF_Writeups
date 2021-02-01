# Bounty Hacker
## This is a pretty simple CTF:  
Name: Alexander Spiesberger  
Date: 1/02/2021  
email: alex.spiesberger@gmail.com   

---

We start and launch an nmap and find 3 ports open, with and ftp on it with anonymous, we connect to it:

![nmap](assets/1.png)
![ftp](assets/ftp.png)

We see 2 documents and download them:

![get](assets/2.png)

We then cat them and see the name of the user:

![cat](assets/3.png)

The other file is a file with passwords, we remember that ssh is open on port 22 so we could try to bruteforce it:

![hydra](assets/4.png)

Yay! We found our password! Let's ssh into it:

![ssh](assets/5.png)

We can now cat user.txt:

![user.txt](assets/6.png)

we try the command "sudo -l" to see if we can run commands as superuser:

![sudo -l](assets/7.png)

We find a nice command that we can run as superuser, we can look at gtfo bins to see what we can do with it:

![gtfobins](assets/8.png)

We now just have to run the command:

![escalate](assets/9.png)

Now we just have to go to /root and cat out the last flag:

![root.txt](assets/10.png)

Hope it was useful.  
contact: alex.spiesberger@gmail.com