# Bounty Hacker: https://tryhackme.com/room/cowboyhacker

Contribution: Andrew Tran

First questions ask us to deploy both the attack machine and lab machine before starting the lab. In this lab I will be using the attackbox provided by tryhackme.

![question1](question1.png)

Second questions has us find open ports on the machine. Network ports are endpoints for software program and network services. To find the open ports we will use nmap which is a reconnaissance tools used to scan a user's ip for ports.

nmap [ip address]

From the scan we're able to find three open ports.

![question2](question2.png)

Let's open FTP to see what we can find.

FTP  [ip address]

Logging in through ftp it seems that there is no credentials needed and that you can login in anoymously which allows us to access the public data on ftp. Opening the directory we can find two files that we can then download onto our machine.

![question3](question3.png)


![download](download.png)

Looking back at the third question it ask who wrote the task list. When we open up the task list we find out who wrote the task list inside of the text.

![tasklist](tasklist.png)

we're also given another file called locks.txt which contains what i presume a list of all the different passwords we can possibly try

![alt text](image.png)

now that we have a username we can try bruteforcinng the ssh using the locktxt we were given. this also answer the next question on what service we can bruteforce. to bruteforce the ssh we will use hydra 

hydra -l lin -P locks.txt <target-ip> ssh

bruteforcing the ssh with the given data gives us a successful login to the ssh
![alt text](image-1.png)

inside of lin's ssh she has one file that is the user flag

![alt text](image-3.png)

now that we have the user flag we need to figure out a way to escalate to root privilage so we can find root flag

the first thing i would do would to see what privelages lin has on her ssh. so i would do: 

sudo -l

this command tell us what root privilegage lin has

![alt text](image-2.png)

It looks likes lin has some sudo privileage for tar so I'll use a website called GTFObin that gives a list of different executable that bypass security restrictions.

![alt text](image-4.png)

I'll then paste this executable into lin's ssh

doing this successfully gives me root 

![alt text](image-5.png)

we can then navigate to the root files where we we can find the root flag 

![alt text](image-6.png)

