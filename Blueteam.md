# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology
_TODO: Fill out the information below._

The following machines were identified on the network:
- Kali
  - **Operating System**: Kali Debian Linux 5.4.0
  - **Purpose**: Attacker / pen test machine
  - **IP Address**: 192.168.1.90
- ELK
  - **Operating System**: Ubuntu 18.04
  - **Purpose**: Elastisearch & Kibana Stack
  - **IP Address**: 192.168.1.100
- Target 1
  - **Operating System**: Debian GNU/Linux 8/v3.16.0-6
  - **Purpose**: WordPress host
  - **IP Address**: 192.168.1.110
- Target 2
  - **Operating System**: Debian GNU/Linux 8/v3.16.0-6
  - **Purpose**: WordPress host
  - **IP Address**: 192.168.1.115
- Capstone
  - **Operating System**: Ubuntu 18.04
  - **Purpose**: The vulnerable Web Server
  - **IP Address**: 192.168.1.105
- hyperV
  - **Operating System**: Windows
  - **Purpose**: Gateway / Hosts
  - **IP Address**: 192.168.1.1

- network Diagram
<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/Final%20RedvsBlue.drawio.png?raw=true" style="height: 400px; width:600px;"/>

### Description of Targets
Out of the vulnerable VMs on the network, Target 1 was the focus of the attack.

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:
nmap -sV -sC -O 192.168.1.110

<img src="https://github.com/mhighbe-20/Cybersecurity_Final_Project/blob/main/Images/RedTeam/Target-1_nmap.png?raw=true" style="height: 400px; width:600px;"/>

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

**`Excessive HTTP Errors`**  is implemented as follows:
  - **Metric**: WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes
  - **Threshold**: IS ABOVE 400
  - **Vulnerability Mitigated**: Enumeration/Brute Force
  - **Reliability**: This alert is high reliability. Measuring by error codes 400 and above will filter out normal or successful responses. High events within the 5 minutes will trigger the alert.
  IMAGE

#### HTTP Request Size Monitor
**`HTTP Request Size Monitor`** is implemented as follows:
  - **Metric**: WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute
  - **Threshold**: IS ABOVE 3500
  - **Vulnerability Mitigated**: Code injection in HTTP requests (XSS and CRLF) or DDOS
  - **Reliability**: Alert could create false positives. It comes in at a medium reliability. There is a possibility for a large non malicious HTTP request or legitimate HTTP traffic.
IMAGE

#### CPU Usage Monitor
**`CPU Usage Monitor`** is implemented as follows:
  - **Metric**: Open Metricbeats WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes
  - **Threshold**: IS ABOVE 0.5
  - **Vulnerability Mitigated**: Malicious software, programs (malware or viruses) running taking up resources
  - **Reliability**: The alert is highly reliable. Even if there isn’t a malicious program running this can still help determine where to improve on CPU usage.
  IMAGE

_TODO Note: Explain at least 3 alerts. Add more if time allows._

### Suggestions for Going Further (Optional)
_TODO_:
- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain _how_ to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 2
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
- Vulnerability 3
  - **Patch**: TODO: E.g., _install `special-security-package` with `apt-get`_
  - **Why It Works**: TODO: E.g., _`special-security-package` scans the system for viruses every day_
