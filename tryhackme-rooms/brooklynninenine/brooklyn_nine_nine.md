# Brooklyn Nine Nine: https://tryhackme.com/room/brooklynninenine

Contribution: Andrew Tran

Investigating the website we find the thumbnail for brooklyn nine nine. When we inspect the element of the page,  we're given a hint that it's related to stenoagraphy. 

![stenography](sten.png)

Stenography is the process of concealing data through harmless looking files. The first place we can analyze is the thumbnail file brooklyn.jpg from the target website

![thumbnail](thumbnail.png)

I was unsure what tool to use to analyze and extract hidden data from an image so I searched up and found a tool called stegcracker which is a bruteforce tool for media files . 

![stegcracker](stegcracker.png)

inside of kali linux are a assortment of different wordlist. for the jpg i will be using the rockyou wordlist which contains a large number of passwords. 

stegcracker brooklyn99.jpg rockyou.txt

using stegcracker reveals a hidden password,  all we need now is a username

![alt text](image.png)

I initally forgot to perform a network scan using Nmap. 

nmap (target-ip)

The three open ports scanned are FTP, SSH, and HTTP which is the website. In order to open the SSH we need a username.  Let's look to see if FTP is anonymous.

ftp (target-ip)

![alt text](image-3.png)

Looking into FTP, we are able to access the files without any credentials. Because FTP is set up as anonymous, the server doesn't require a account

![alt text](image-1.png)

Inside FTP we find a file called note_to_jake.txt

![alt text](image-2.png)

Inside of the txt file we find three names. Amy, Jake, and Holt.

![alt text](image-4.png)

Now that we have names, we can try plugging them into SSH. I'll first try Jake since the note asked jake to change his password.

![alt text](image-5.png)

When I enter the password into jake's ssh it ends up being wrong. So I tryed it for the other two user which also didn't work. Maybe I could try bruteforcing the SSH since amy said that jake's password is weak. For this bruteforce I will be using hydra which is a network password cracker tool. I will use jake as the user and rockyou as the password list. 

hydra -l jake -P rockyou.txt <target-ip> ssh

From hydra I was able to uncover a password for jake

![alt text](image-6.png)

Using the password I was able to login into jake's ssh successfully

![alt text](image-7.png)

using ls reveals no files, but if we back out of the directory we can find three different folders. when we look inside of holt's folder we find two files one of which contains the flag for the first question

![alt text](image-8.png)

the second question ask us to find a the flag for the root user. somehow we need to escalate our level to root. first thing we can do is see what root commands we can access without a password

when we check what access we have, it shows us that we can run /usr/bin/less.

![alt text](image-9.png)

the first thing i did was searched up any vulnerabilties with this command line. one of the first website that popped up was GTFOBins which has a list of different executables that can bypass security restrictions from misconfigured systems. 

![alt text](image-11.png)

pasting these commands into jake's ssh successfully gives us root access. and we're finally able to find the root flag

![alt text](image-12.png)


Tools used:

Nmap - port scanning

Stegcracker - media file bruteforce 

Hydra - network bruteforce

Doing this CTF I learned more about how to escalate to root. One of the commands that I found really useful was sudo -l which tells you what sudo actions you have access to. This allowed me to search for the vulnerability online giving me root access. I also learned a little bit about the interactive file viewer less and how to use it. The one important thing I learned from this practice CTF is how to strucuture my attack. Doing reconnisanse first then enumaration then exploitation.