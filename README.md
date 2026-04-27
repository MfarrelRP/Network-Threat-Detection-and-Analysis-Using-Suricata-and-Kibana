# Overview
This project implements a **Network Intrusion Detection System (IDS)** on a Debia-Based Linux Environment. It leverages:

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
