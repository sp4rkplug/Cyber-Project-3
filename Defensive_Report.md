# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- Capstone
  - **Operating System**: Linux
  - **Purpose**: Web Server
  - **IP Address**: 192.168.1.105
- ELK
  - **Operating System**: Linux
  - **Purpose**: Logging server using ELK Stack
  - **IP Address**: 192.168.1.100
- Kali
  - **Operating System**: Kali Linux
  - **Purpose**: Penetration Testing machine
  - **IP Address**: 192.168.1.90
- Target 1
  - **Operating System**: Linux
  - **Purpose**: Wordpress host server
  - **IP Address**: 192.168.1.110

### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

Excessive HTTP Errors is implemented as follows:
  - **Metric**: top 5 HTTP response codes
  - **Threshold**: above 400 for the last 5 minutes
  - **Vulnerability Mitigated**: brute force attacks
  - **Reliability**: Large amounts of HTTP errors indicate either a server issue or a possible attack; therefore, the reliability would be high.

#### HTTP Request Size Monitor
HTTP Request Size Monitor is implemented as follows:
  - **Metric**: size of HTTP requests in bytes over all documents
  - **Threshold**: above 3500 for the last 1 minute
  - **Vulnerability Mitigated**: DDoS attacks, code injection
  - **Reliability**: Request sizes average lower, but can exceed 3500 on occasion.  This would be rare enough to check out, though.  I would put this one at medium reliability.

#### CPU Usage Monitor
CPU Usage Monitor is implemented as follows:
  - **Metric**: CPU usage across all documents
  - **Threshold**: above 50% for the last 5 minutes
  - **Vulnerability Mitigated**: malicious software installation
  - **Reliability**: Fifty percent CPU usage over 5 minutes would definately indicate an issue.  This is high reliability.
