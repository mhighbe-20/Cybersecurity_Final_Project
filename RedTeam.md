# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services
_TODO: Fill out the information below._

Nmap scan results for each machine reveal the below services and OS details:

```bash
Command: $ nmap -sV -sC -O 192.168.1.0/24
  ```
 <img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/Target-1_nmap.png" alt="Target1_IP" style="height: 400px; width:600px;"/>


This scan identifies the services below as potential points of entry:
   Target 1  
     1. Port 22/TCP Open SSH       
     2. Port 80/TCP Open HTTP       
     3. Port 111/TCP Open rcpbind       
     4. Port 139/TCP Open netbios-ssn     
     5. Port 445/TCP Open netbios-ssn  

_TODO: Fill out the list below. Include severity, and CVE numbers, if possible._

The following vulnerabilities were identified on each target:
   Target 1  
    1. User Enumeration (WordPress site)  
       a. The stop-user-enumeration plugin before 1.3.8 for WordPress has XSS. [CVE-2017-18536](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-18536/ "CVE-2017-18536")  
       b. WordPress Core < 4.7.1 - Username Enumeration Vulnerability CVE-2017-5487 Scanner [CVE-2017-5487](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5487/ "CVE-2017-5487")  
    2. Weak user Passwords  
    3. Unsalted user Password Hash (WordPress)  
    4. Misconfiguration of User Privileges/Privilege Escalation  

_TODO: Include vulnerability scan results to prove the identified vulnerabilities._

### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _TODO: Insert `flag1.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
