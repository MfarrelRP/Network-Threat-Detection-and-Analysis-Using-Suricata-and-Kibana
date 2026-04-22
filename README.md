# Installing
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
