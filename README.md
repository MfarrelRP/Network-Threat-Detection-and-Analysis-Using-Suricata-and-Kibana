# Overview
This project implements a **Network Intrusion Detection System (IDS)** on a Ubuntu-Based Linux Environment. It leverages:

- **Suricata** - for real-time network traffic monitoring and threat detection
- **Elasticsearch** - for storing and indexing log data
- **Filebeat** - for forwading Suricata logs to ELasticseach
- **Kibana** - for Visualizing and analyzing detected threats via a web dashboard

The system is designed in complience with the **IA Triad (Confidentiality, Integrity, Availability) to ensure secure, reliable, and accessible threat monitoring.

# **Suricata Installation and Configuration**
**1. Installation**

First, we need to update the systems repositories and install essential utilities to ensure the latest package are availabe using the bash below: 
```
sudo apt update
sudo apt install suricata –y
```

**2. Verify installation and configuration**

After installation, verify the suricata version using the bash below:
```
suricata --build-info
sudo systemctl enable suricata
sudo systemctl start suricata
```
The Configuration of Suricata, use the bash below:
```sudo nano /etc/suricata/suricata.yaml```

**3. Network Interface Configuration**

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

- interface: eth0 → Change the interface to the one that we are using (ip a to
check).
- threads: auto → Suricata apart to the amount of CPU thread.
- defrag: yes → activation of IP package defraging.

**4. Logging and Output**

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

- eve.json is the main fle to note all the result of the network trafficking (alert, dns,
http, tls, ssh).
- Format JSON ease the analisys and integration with tools such as Kibana, Splunk, or
Grafana.

**5. Konfigurasi Rules (Detection Rules).**

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

- alert icmp → shows an alert if there is a ping activity.
- alert tcp ... 22 →Detecting SSH access.
- alert tcp ... 80 → Detecting the access to Web Server.

**6. Running the Suricata**

after the configuration is done, run the bash below to run suricata:
```
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl status suricata
```
to see the log without the interface, use the bash:
```tail -f /var/log/suricata/fast.log```

**7. Configuration Verification**

before being run permanently, run a configuration verification:
```udo suricata -T -c /etc/suricata/suricata.yaml -v```
if the output "Configuration provide was successfully loaded" it means the configuration is correct and ready to run.

# Elasticsearch Installation and Configuration
**1. Basic Installation**

First we add Elasticsearch GPG key and repository, add the official repository index, and then install the Elasticsearch by using the bash below:
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg
--dearmor -o /usr/share/keyrings/elastic-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg]
https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee
/etc/apt/sources.list.d/elastic-8.x.list
```
```sudo apt install elasticsearch -y```

**2. Elasticsearch configuration**

after installing, we want to open the configuration using:
```sudo nano /etc/elasticsearch/elasticsearch.yml```

For example, configuration:
```
Cluster and node settings:
cluster.name: elastic-cluster
node.name: debian-node-1

Network Settings:
network.host: 0.0.0.0
http.port: 9200

Path settings:
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
```
**Explanation:**

- cluster.name: → Defines the name of the cluster (useful if you ever scale to multiple
Elasticsearch nodes).
- node.name: → The name of this machine’s node within the cluster.
- network.host: → Setting this to 0.0.0.0 makes Elasticsearch accessible from any IP
address (important for connecting from another VM or Kibana).
- http.port: → Default HTTP port (9200) for REST API communication.
- path.data: and path.logs: → Default directories for database files and logs.

**3. Enable and start the Elasticsearch**

after configuration is done, we can now enable and start the elasticsearch by using the bash below:
```
sudo systemctl daemon-reload
sudo systemctl enable elastichsearch
sudo systemctl start elasticsearch
```

**4. Verify installation and Connection**

after a few seconds, verify that elasticsearch is running and listening on port 9200 using bash:
```curl -X GET "localhost:9200"```

If it works, we will see JSON output similar to this:
```
{
 “name”: “debian-node-1”,
 “cluster_name”: “elastic-cluster”,
 “cluster_uuid”: “Yt1s9Lr1...”
 “version”: {
 “number”: “8.15.0”,
 “build_flavor”: “default”
 },
 “tagline”: “You Know, for Search”
}
```

**5. Test Access from browser**

Open the browser of the debian, and then try:
```http://<elasticsearch-server-ip>:9200 or http://localhost:9200```
