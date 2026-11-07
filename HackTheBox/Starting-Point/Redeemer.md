# HackTheBox Starting Point: Redeemer

**Platform:** HackTheBox (Starting Point, Tier 0)

**Difficulty:** Very Easy

**Focus:** Redis enumeration, unauthenticated access

## Machine Info
Redeemer is a very easy Linux machine which explores the enumeration and exploitation of a Redis database server while showcasing the redis-cli command line utility and basic commands to interact with the Redis service.

## Setup
This was the second time I used openvpn, it has been almost a week since I last used it so, to refresh my memory I googled the command, which gave me this simple output to use `sudo openvpn [filename].ovpn`. After connecting I made sure everything was in order, with the command `ip a`. And everything was alright, so I was ready to start.

## Enumeration and research
I wanted to see what ports were open, so I used the command ``nmap -p- [ip address]``. I waited for a while but it was taking forever, so I took a hint from hackthebox which gave me a better insight for my case, specifically to add --min-rate 5000, which significantly sped up the process. 
After doing that, I found out on port 6379 a service named ``Redis`` was running. I've never heard of it. I wanted to see what it was used for. After doing some research I found out it was a service for storing data on ram, for it to be faster. I found that to be a very interesting concept.
I learned how to use Redis, and discovered some basic commands like ``redis-cli`` to interact with the server, ``select`` to switch databases, ``-h`` to specify the hostname, and ``info`` to see details about the server. Later on, I also found out that it was running version 5.0.7.

## Exploitation
Firstly, I used the information learned earlier to connect to the server using ``redis-cli -h 10.129.104.134``. I selected the database and found out there are only 4 keys. I've obtained the keys with the ``KEYS *`` command and moved to the final steps. I discovered the ``flag`` key and entered the commands ``TYPE flag`` and ``GET flag`` which returned me the flag.

## What I learned
- I refreshed my memory about the connection to openvpn
- I learned new syntaxes for nmap to speed up the port scanning process
- I learned what Redis is, and what port it uses
- I learned some basic commands for Redis

## Remediation
- Require authentication for Redis (set `requirepass` in the config file)
- Don't expose Redis directly to the internet — restrict it to localhost or a trusted internal network
- Use a firewall to block external access to port 6379

