# Overview
This project implements a **Network Intrusion Detection System (IDS)** on a Ubuntu-Based Linux Environment. It leverages:

- **Suricata** - for real-time network traffic monitoring and threat detection
- **Elasticsearch** - for storing and indexing log data
- **Filebeat** - for forwading Suricata logs to ELasticseach
- **Kibana** - for Visualizing and analyzing detected threats via a web dashboard

The system is designed in complience with the **IA Triad (Confidentiality, Integrity, Availability) to ensure secure, reliable, and accessible threat monitoring.

# **Suricata Installation and Configuration**
# Installation
First, we need to update the systems repositories and install essential utilities to ensure the latest package are availabe using the bash below: 
```
sudo apt update
sudo apt install suricata –y
```

# Verify installation and configuration
After installation, verify the suricata version using the bash below:
```
suricata --build-info
sudo systemctl enable suricata
sudo systemctl start suricata
```
The Configuration of Suricata, use the bash below:
```sudo nano /etc/suricata/suricata.yaml```

# Network Interface Configuration
Look for this part on the configuration:
```
af-packet:
 - interface: eth0
 threads: auto
 cluster-id: 99
 cluster-type: cluster_flow
 defrag: yes
```
**Explanation**:
• interface: eth0 → Change the interface to the one that we are using (ip a to
check).
• threads: auto → Suricata apart to the amount of CPU thread.
• defrag: yes → activation of IP package defraging.

# Logging and Output
Make sure this section is active, so that the logs can be analyzed
```
outputs:
 - eve-log:
 enabled: yes
 filetype: regular
 filename: /var/log/suricata/eve.json
 community-id: yes
 types:
 - alert
 - dns
 - http
 - tls
 - ssh
```
**Explanation**
• eve.json is the main fle to note all the result of the network trafficking (alert, dns,
http, tls, ssh).
• Format JSON ease the analisys and integration with tools such as Kibana, Splunk, or
Grafana.

# Konfigurasi Rules (Detection Rules).
The Rules File usually is located in
```/etc/suricata/rules/suricata.rules```

For base example (you can add this at the end of file)
```
alert icmp any any -> any any (msg:"ICMP Ping Detected"; sid:1000001;
rev:1;)
alert tcp any any -> any 22 (msg:"SSH Access Detected"; sid:1000002;
rev:1;)
alert tcp any any -> any 80 (msg:"HTTP Access Detected"; sid:1000003;
rev:1;)
```
**Explanation**
• alert icmp → shows an alert if there is a ping activity.
• alert tcp ... 22 →Detecting SSH access.
• alert tcp ... 80 → Detecting the access to Web Server.

# Running the Suricata
after the configuration is done, run the bash below to run suricata:
```
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl status suricata
```
to see the log without the interface, use the bash:
```tail -f /var/log/suricata/fast.log```

# Configuration Verification
before being run permanently, run a configuration verification:
```udo suricata -T -c /etc/suricata/suricata.yaml -v```
if the output "Configuration provide was successfully loaded" it means the configuration is correct and ready to run.
