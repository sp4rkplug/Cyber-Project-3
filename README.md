# Activity File: Alert and Attacking Target 1

## Overview

You are working as a Security Engineer for X-CORP, supporting the SOC infrastructure. The SOC analysts have noticed some discrepancies with alerting in the Kibana system and the manager has asked the Security Engineering team to investigate. 

To start, your team needs to confirm that newly created alerts are working. Once the alerts are verified to be working, you will monitor live traffic on the wire to detect any abnormalities that aren't reflected in the alerting system. 

You will then report back all your findings to both the SOC manager and the Engineering Manager with appropriate analysis.

You have two class days to complete this activity.

## Instructions

Start by configuring new alerts in Kibana. Once configured, you will test them by attacking a system.

Open the [Defensive Report](https://github.com/chefesteve/Final_Project/blob/main/Defensive_Report.md) and complete it as you progress through the activity.

### Configuring Alerts

Complete the following to configure alerts in Kibana:

1.  Access Kibana at `192.168.1.100:5601`

2. Click on **Management** > **License Management** and enable the Kibana Premium Free Trial.

3. Click **Management** > **Watcher** > **Create Alert** > **Create Threshold Alert**

4. Implement three of the alerts you designed at the end of Project 2.

You can configure any alerts you'd like, but you are recommended to start with the following:

- **Excessive HTTP Errors**
  - Select the **packetbeat** indice

  ```kql
  WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes
  ```
  
- **HTTP Request Size Monitor**
  - Select the **packetbeat** indice

  ```kql
  WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute
  ```

- **CPU Usage Monitor**
  - Select the **metricbeat** indice

  ```kql
  WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes
  ```

#### Viewing Logs Messages  

There are a few way to to view these log messages and their associated data options. 

- First, you can see when alerts are firing directly from the Watcher screen.

- As you attack Target 1, keep the watcher page open to view your alerts fire in real time.

   ![](images/Alert.png)

- To view network traffic associated with these messages, we need to create a new `Index Pattern`:

- Click on **Management > Index Patterns** and click on the button for `Create Index Pattern`.
   
   ![](images/IndexPatterns.png)
   ![](images/CreateIndex.png)

- Make sure to turn on the toggle button labeled `Include System Indices` on the top right corner.

    ![](images/includeIndices.png)

- Create the pattern `.watcher-history-*`.

   ![](images/defineWatcherPattern.png)

- After you have this new index pattern, you can search through it using the `Discovery` page.
   ![](images/discovery.png)

- Enter `result.condition.met` in as search filter to see all the traffic from your alerts.

   ![](images/discovery-filter.png)
   ![](images/alert-traffic.png)

### Attacking Target 1

Open the [Offensive Report](https://github.com/chefesteve/Final_Project/blob/main/Offensive_Report.md) and complete it while you progress this activity.

You will need to run a few commands on `Target 1` in order to ensure that it forwards logs to Kibana. Follow the steps below:

- Open the Hyper-V Manager.
- Connect to `Target 1`.
- Log in with username `vagrant` and password `vagrant`.
- Escalate to `root` with `sudo -s`.
- Run `/opt/setup`.

This enables Filebeat, Metricbeat, and Packetbeat on the Target VM if they are not running already.

Now that you've configured alerts, you'll attack the `Target 1` vulnerable VM on this network. 

**Note**: Ignore the `Target 2` machine at this time. If you complete the entire project with time to spare, ask your instructor for directions on attacking Target 2 and integrating it into your project.

Complete the following steps:

1. Scan the network to identify the IP addresses of Target 1.

2. Document all exposed ports and services.

3. Enumerate the WordPress site. One flag is discoverable after this step.
     - **Hint**: Look for the `Users` section in the output.

4. Use SSH to gain a user shell. Two flags can be discovered at this step.

     - **Hint**: Guess `michael`'s password. What's the most obvious possible guess?

5. Find the MySQL database password.
     - **Hint**: Look for a `wp-config.php` file in `/var/www/html`.

6. Use the credentials to log into MySQL and dump WordPress user password hashes.

7. Crack password hashes with `john`.
     - **Hint**: Start by creating a wp_hashes.txt with Steven and Michael's hashes, formatted as follows

      ```bash
      user1:$P$hashvalu3
      user2:$P$hashvalu3
      ```

8. Secure a user shell as the user whose password you cracked.

9. Escalate to `root`. One flag can be discovered after this step.
    - **Hint**:  Check sudo privileges. Is there a python command you can use to escalate to sudo?

Try to complete all of these steps. However, you may move on after capturing only _two_ of the four flags if you run out of time.

---

© 2021 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.






## Activity File: Wireshark Strikes Back

### Overview

You are working as a Security Engineer for X-CORP, supporting the SOC infrastructure. The SOC analysts have noticed some discrepancies with alerting in the Kibana system and the manager has asked the Security Engineering team to investigate. 

Yesterday, your team confirmed that newly created alerts are working. Today, you will monitor live traffic on the wire to detect any abnormalities that aren't reflected in the alerting system. 

You are to report back all your findings to both the SOC manager and the Engineering Manager with appropriate analysis.

Fill out the [Network Report](https://github.com/chefesteve/Final_Project/blob/main/Network_Report.md) as you progress through this activity.

### Setup

You will use the Kali VM to analyze live traffic on the wire.

In order to get started, you will need to:
- Connect to the Kali VM.
- Open a terminal window and run the command `systemctl start sniff`. 
    - This command uses `tcpreplay` to replay PCAPs in `/opt/pcaps` onto Kali’s `eth0` interface. 
- Launch Wireshark and capture traffic on the `eth0` interface.
- After 15 minutes have passed, run the command `systemctl stop sniff` to stop the `tcpreplay`. 
  - Please note that replaying the PCAPs will use up the CPU memory. You will need to stop this service in order to avoid performance issues with your virtual machine. 
- Save the capture to file. (**This is an important step**.)
- Profile users' behavior from their packet data.

If you are unable to find some of the solutions, it is possible you did not allow Wireshark to capture traffic for long enough. To save time, feel free to use the following PCAP file below to answer the questions:
  
  - PCAP 
  
**Note:** You will be dealing with live malware in this activity. Please make sure all work is done on Azure machines. 

### Instructions

Connect to your Kali VM, launch Wireshark, and begin capturing on the `eth0` interface using the steps above. Let the capture run for at least fifteen minutes and then stop it. As your capture runs, read the following overview: 

- The Security team requested this analysis because they have evidence that people are misusing the network. Specifically, they've received tips about:
    - "Time thieves" spotted watching YouTube during work hours.
    - At least one Windows host infected with a virus.
    - Illegal downloads.

- A number of machines from foreign subnets are sending traffic to this network. Therefore, you will see many IP ranges in your capture. 

- Your task is to collect evidence confirming the Security team's intelligence. 

- Be sure to use display filters. In addition, be sure to record your work by adding comments to packets as you go. 

  - For example, if you find a packet containing a username of interest, comment on the packet: `Illegal Downloads: Contains Windows username`.

Record your answers in the following Google Doc. This file will be submitted as a deliverable at the end of the project. You must make a copy of this file in order to edit it.

- [Network Analysis Report](https://github.com/chefesteve/Final_Project/blob/main/Network_Report.md)

#### Time Thieves

At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:

- They have set up an Active Directory network.
- They are constantly watching videos on YouTube.
- Their IP addresses are somewhere in the range `10.6.12.0/24`.

You must inspect your traffic capture to answer the following questions in your Network Report:
1. What is the domain name of the users' custom site?
2. What is the IP address of the Domain Controller (DC) of the AD network?
3. What is the name of the malware downloaded to the 10.6.12.203 machine?
   - Once you have found the file, export it to your Kali machine's desktop.

4. Upload the file to [VirusTotal.com](https://www.virustotal.com/gui/). 
5. What kind of malware is this classified as?

#### Vulnerable Windows Machines

The Security team received reports of an infected Windows host on the network. They know the following:
- Machines in the network live in the range `172.16.4.0/24`.
- The domain mind-hammer.net is associated with the infected computer.
- The DC for this network lives at `172.16.4.4` and is named Mind-Hammer-DC.
- The network has standard gateway and broadcast addresses.

Inspect your traffic to answer the following questions in your network report:

1. Find the following information about the infected Windows machine:
    - Host name
    - IP address
    - MAC address
    
2. What is the username of the Windows user whose computer is infected?
3. What are the IP addresses used in the actual infection traffic?
4. As a bonus, retrieve the desktop background of the Windows host.


#### Illegal Downloads

IT was informed that some users are torrenting on the network. The Security team does not forbid the use of torrents for legitimate purposes, such as downloading operating systems. However, they have a strict policy against copyright infringement.

IT shared the following about the torrent activity:

- The machines using torrents live in the range `10.0.0.0/24` and are clients of an AD domain.
- The DC of this domain lives at `10.0.0.2` and is named DogOfTheYear-DC.
- The DC is associated with the domain dogoftheyear.net.

Your task is to isolate torrent traffic and answer the following questions in your Network Report:

1. Find the following information about the machine with IP address `10.0.0.201`:
    - MAC address
    - Windows username
    - OS version

2. Which torrent file did the user download?


---
© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.  
