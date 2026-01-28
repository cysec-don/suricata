Suricata Configuration
This repository contains a custom configuration file for Suricata, a high-performance Network Intrusion Detection (IDS), Prevention (IPS), and Network Security Monitoring (NSM) engine.

This configuration is generated for Suricata version 8.0.3.

📋 Overview
This configuration file (suricata.yaml) is set up to monitor traffic on specific Linux interfaces (both a virtual bridge and a wireless interface) and logs extended metadata for HTTP, DNS, and TLS protocols.

Key Configuration Details
1. Network Variables
The configuration is tuned for a local environment consisting of two subnets:

HOME_NET: 192.168.122.0/24 (Common for virtualization/libvirt) and 192.168.1.0/24 (Common for home/private LANs).
EXTERNAL_NET: Defined as !$HOME_NET (everything else).
2. Capture Interfaces (AF-PACKET)
The system is configured to use af-packet for high-speed capture on Linux. It listens on the following interfaces:

virbr0:
Threads: 1 (Configured for low-traffic VM bridge).
Cluster ID: 98.
wlp0s20f3 (Wireless Interface):
Threads: auto (Detects core count automatically).
Cluster ID: 99.
3. Logging & Outputs
The configuration enables several distinct logging outputs:

EVE-JSON (eve.json): The primary Extensible Event Format log.
Extended Logging: Enabled for HTTP, DNS, and TLS to capture detailed metadata (e.g., user agents, request URIs, TLS certificates).
Community ID: Enabled for flow correlation with other tools.
Anomaly Logging: Enabled to detect decode/stream/application layer issues.
DHCP Logging: Enabled to map MAC addresses to IPs.
Fast Log (fast.log): A compact, line-based alert log.
Stats Log (stats.log): Performance and traffic statistics.
4. Application Layer Protocols
Parsing is enabled for a wide variety of protocols including HTTP, DNS, TLS, SSH, SMTP, SMB, FTP, and MQTT.

🚀 Usage
To use this configuration file:

1. Install Suricata
Ensure you have Suricata installed on your Linux system.

bash

sudo apt-get install suricata
# or
sudo yum install suricata
2. Backup your existing configuration
bash

sudo cp /etc/suricata/suricata.yaml /etc/suricata/suricata.yaml.bak
3. Apply this configuration
Copy the content of suricata.txt (or rename the file to suricata.yaml) to your Suricata configuration directory.

bash

sudo cp suricata.yaml /etc/suricata/
4. Validate the configuration
Before running, check for syntax errors:

bash

sudo suricata -T -c /etc/suricata/suricata.yaml -v
5. Run Suricata
Run Suricata pointing to this configuration file. Since the config specifies interfaces virbr0 and wlp0s20f3 inside the file, Suricata will attempt to listen on those automatically.

bash

sudo suricata -c /etc/suricata/suricata.yaml
Alternatively, you can run it on a specific interface defined in the file:

bash

sudo suricata -c /etc/suricata/suricata.yaml -i virbr0
📂 File Structure
suricata.yaml: The main configuration file.
default-log-dir: /var/log/suricata/ (This is where logs will be written).
⚙️ Customization
To customize this file for your environment:

Change IP Ranges: Edit the HOME_NET variable under the vars section to match your actual network range.
Update Interfaces: If your network interface names differ from virbr0 or wlp0s20f3, update the af-packet section.
Manage Rules: This configuration handles the engine settings. Ensure you download the latest rule sets (Suricata-Update) if you want active detection alerts.
⚠️ Version Compatibility
This file specifies suricata-version: "8.0". Using this configuration with significantly older or newer versions of Suricata may result in errors due to deprecated options or missing new features.

📄 License
Please refer to the Suricata License regarding the usage of the engine. This specific configuration file is provided as-is for setup purposes.

📚 Documentation
For more details on Suricata configuration options, visit the Official Suricata Documentation.
