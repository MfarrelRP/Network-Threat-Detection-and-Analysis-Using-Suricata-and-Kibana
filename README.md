# Overview
This project implements a **Network Intrusion Detection System (IDS)** on a Ubuntu-Based Linux Environment. It leverages:

- **Suricata** - for real-time network traffic monitoring and threat detection
- **Elasticsearch** - for storing and indexing log data
- **Filebeat** - for forwading Suricata logs to ELasticseach
- **Kibana** - for Visualizing and analyzing detected threats via a web dashboard

The system is designed in complience with the **IA Triad (Confidentiality, Integrity, Availability) to ensure secure, reliable, and accessible threat monitoring.

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
