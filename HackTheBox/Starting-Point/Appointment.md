# HackTheBox Starting Point: Appointment

**Platform:** HackTheBox (Starting Point, Tier 0)

**Difficulty:** Very Easy

**Focus:** Web enumeration, Gobuster, SQL Injection (Authentication Bypass)

## Machine Info
Appointment is a very easy Linux machine which showcases beginner SQL Injection techniques against an SQL database enabled web application.

## Setup
Today, I used openvpn once again and it started to feel pretty familiar to me. Without hesitation I opened the terminal, typed `sudo openvpn [filename].ovpn`. And I was ready to go

## Questions & Concepts
Before jumping straight to the enumeration and exploitation chapter I want to talk about the questions because they were a good chunk of this box.

- **SQL & SQL Injection:** I heard about this concepts prior in my tryhackme journey but to refresh my brain again I found out that SQL stands for (`Structured Query Language`). And one of its most common vulnerabilities is SQL injection.
- **OWASP Top 10:** Next, I checked where this vulnerability sits in the 2021 OWASP Top 10 list, which is `A03:2021-Injection`
- **Web & HTTP Basics:** This was the last step before actually opening the terminal. HTTPS runs on port 443 and a folder called in web-application terminology is directory.

## Enumeration and research
Firstly, I wanted to see what ports were open so I checked that with the command `nmap -sV <TARGET_IP>` and the result was 
``PORT       STATE   SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))``
Since port `80` was open and running an Apache web server, I opened `http://<TARGET_IP>` in my browser and found a login page.

Since this was about SQL Injection this was really it for this chapter and now it is time to move on to the exploitation.

## Exploitation
I quickly learned that the `#` a character so simple can make you login as an admin, without requiring a password
So I went to the login page and entered:
- **Username:** `admin'#` (or `admin' #`)
- **Password:** `anything` (it gets ignored because of the `#` comment)
After submitting the form, the login was bypassed and I was redirected to a page starting with the word `Congratulations`, where the flag was displayed right on the screen. I copied it, pasted it and the box was done.

## What I learned
- What SQL stands for (Structured Query Language) and how SQL Injection works
- Where Injection sits in the 2021 OWASP Top 10 (`A03:2021-Injection`)
-- How the `#` character is used and how can you use SQL Injection with it.

## Remediation
-Check and filter user input: Validate input on the server side so unexpected characters or weird patterns don't slip through unchecked.
--Hash passwords properly: Store hashed passwords instead of plain text, and verify them securely in the application code rather than letting an unsafe SQL query decide the login.


