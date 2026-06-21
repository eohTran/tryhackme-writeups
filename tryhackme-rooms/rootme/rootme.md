# RootMe

[RootMe](https://tryhackme.com/room/rrootme) - description later

### Table of contents:
* [Deploy Machine](#Deploy-Machine)
    * [OpenVPN](#openvpn)
* [Enumeration](#enumeration)
    * [Port Scanning](#port-scanning)
    * [SSH Enumerartion](#ssh-enumeration)
    * [Directory Enumeration](#directory-enumeration)
    * [HTTP Enumeration](#http-enumeration)
* [Reverse Shell](#reverse-shell)

## Deploy Machine

### OpenVPN

For this room, I will be connecting using OpenVPN. You can find the setup guide on the tryhackme settings

![alt text](image.png)

To properly setup the VPN, to connect to the target machine, you will need both OpenVPN and the configuration files provided in the setup guide

![alt text](image-1.png)

If you're on a linux machine all you need to do now is go onto the terminal and locate the configuration file and run  `sudo openvpn [file location]`

![alt text](image-2.png)

Now you're ready to start working on the room!

## Enumeration
### Port Scanning

We first need to scan the machine, and we can do that using Nmap.

`nmap [ip address]`

From the Nmap scan there are 2 open ports: 22(SSH) and 80(HTTP)

To find out what version of Apache is running, we can add the parameter `-sV` which stands for service version. This can tell us what version the ports are running on. And from the new Nmap scan the appache version is 2.4.41

![alt text](image-3.png)

### SSH Enumeration
Port 22 is the SSH, to login into a ssh we need a username, hostname(website/ip), and password

``ssh username@hostname``

``password:``

However since we have no credential we will have to look else where before we can start getting into the SSH.
### Directory Enumeration
Before we start looking at port 80(HTTP), we need to conduct a directory enumeration via GoBuster as asked by the room. Since I've never touched GoBuster I'll do a quick search on the basic formatting.

![alt text](image-4.png)

Since I'm using Kali Linux, I will be using the wordlist provided, however you can do a quick google search for directry files to use for GoBuster.

![alt text](image-5.png)

After just a few seconds, we successfully found 4 subdirectories: upload, css, js, and panel.

![alt text](image-6.png)

### HTTP Enumeration
Going through each subdirectory, the only one that looks weird is panel. From the subdirectory it looks custom-made which could provide clues on what to do next.

![alt text](image-7.png)

If I try pressing the upload button is presents a text in spanish which translates to "Error sending the file". Inspecting the element I realized that the main page shows the ssh which is ``root@rootme``

##  Reverse Shell

Going back to the room, it asks us to find a form to upload and get a reverse shell. Let's go ahead and test different forms to see which one works with the websitel. We can start with PHP to see if they have it enabled on the server