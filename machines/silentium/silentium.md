
# Silentium - Hack the Box Writeup
## Overview
- __Machine Name__ : Silentium
- __Platform__ : Hack the Box
- __IP__ : 10.129.42.226
- __Difficulty__ : Easy
- __OS__ : Linux
- __Date__ : 21 April 26

## Objective
Finding User Flag
Finding Root Flag

## Reconnassiance
### Nmap Scan
nmap -sC -sV 10.129.34.96

  **Findings**
  - **22/tcp(SSH)**
  
      - Potential for remote access (Require credentials for access)
  - **80/tcp(HTTP)**
  
      - Web Server detected.
  
      - Primary attack surface identified.
## Web Enumeration
### Directory Bruteforcing


