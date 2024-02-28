# Network Forensic Analysis Report


## Time Thieves 
You must inspect your traffic capture to answer the following questions:

1. What is the domain name of the users' custom site?
	frank-n-ted.com
	
	![domain](https://github.com/chefesteve/Final_Project/blob/main/screens/network/frank-n-ted.png)
	
	
2. What is the IP address of the Domain Controller (DC) of the AD network?
	10.6.12.12
	
	![IP](https://github.com/chefesteve/Final_Project/blob/main/screens/network/ad_ip.png)
	
	
3. What is the name of the malware downloaded to the 10.6.12.203 machine?
	june11.dll
	
	![malware](https://github.com/chefesteve/Final_Project/blob/main/screens/network/malware.png)
	
	
   - Once you have found the file, export it to your Kali machine's desktop.
4. Upload the file to [VirusTotal.com](https://www.virustotal.com/gui/). 
5. What kind of malware is this classified as?
	trojan
	
	![malware](https://github.com/chefesteve/Final_Project/blob/main/screens/network/trojan.png)
	
	

---

## Vulnerable Windows Machine

1. Find the following information about the infected Windows machine:
    - Host name
		ROTTERDAM-PC
		
		![Infected Host](https://github.com/chefesteve/Final_Project/blob/main/screens/network/infected.png)
		
		
    - IP address
		172.16.4.205
		
		![IP Address](https://github.com/chefesteve/Final_Project/blob/main/screens/network/rot_ip.png)
		
		
    - MAC address
		00:59:07:b0:63:a4
		
		![MAC](https://github.com/chefesteve/Final_Project/blob/main/screens/network/rot_mac.png)
		
		
    
2. What is the username of the Windows user whose computer is infected?
	matthijs.devries
	
	![user](https://github.com/chefesteve/Final_Project/blob/main/screens/network/matt.png)
	
	
3. What are the IP addresses used in the actual infection traffic?
	185.243.115.84 and 166.62.111.64
	
	![IP](https://github.com/chefesteve/Final_Project/blob/main/screens/network/166.png)
	
	![IP](https://github.com/chefesteve/Final_Project/blob/main/screens/network/185.png)
	
	

---

## Illegal Downloads

1. Find the following information about the machine with IP address `10.0.0.201`:
    - MAC address
		00:16:17:18:c8
		
		![MAC](https://github.com/chefesteve/Final_Project/blob/main/screens/network/elmer_mac.png)
		
		
    - Windows username
		elmer.blanco
		
		![user_name](https://github.com/chefesteve/Final_Project/blob/main/screens/network/elmer.png)
		
		
    - OS version
		Windows NT 10.0
		
		![OS](https://github.com/chefesteve/Final_Project/blob/main/screens/network/user.png)
		
		

2. Which torrent file did the user download?
	Betty_Boop_Rhythm_on_the_Reservation.avi.torrent
	
	![torrent](https://github.com/chefesteve/Final_Project/blob/main/screens/network/torrent.png)
