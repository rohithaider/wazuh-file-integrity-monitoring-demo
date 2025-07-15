# wazuh-fim-attack-demo
# Wazuh File Integrity Monitoring (FIM) - Attack Demonstration

This repository contains configuration files, sample logs, and step-by-step procedures to demonstrate File Integrity Monitoring (FIM) using Wazuh. It simulates real-world attacks to showcase how Wazuh detects file modifications, additions, and deletions on endpoints.

## Objective

To provide a clear and repeatable demonstration of Wazuh’s FIM capability by executing various file-based attacks and observing alert generation within the Wazuh dashboard.

---

## Environment Setup

- **Wazuh Manager**: Hosted on a cloud server (e.g., Linode, AWS). I hosted my cloud server in Linode.
  A Documentation for deploying Wazuh Manager can be found here:
  
  ```url
  https://github.com/rohithaider/wazuh-docker-siem-setup
  ```
- **Wazuh Agent**: Installed on Ubuntu VM & Windows 7 (VirtualBox)
- **Version**: Wazuh 4.12+
- **Log Collection**: Direct agent-to-manager communication (no Filebeat)

---

## Wazuh Configuration for File Integrity Monitoring in Ubuntu.
First edit the ossec.conf file in the Wazuh Agent to monitor the root directory. 

```bash
sudo nano /var/ossec/etc/ossec.conf
```
then add this in the <syscheck> block:

```bash
<directories check_all="yes" report_changes="yes" realtime="yes">/root</directories>
```

<img width="571" height="410" alt="image" src="https://github.com/user-attachments/assets/8d39fa9b-97c7-4438-ac45-10b45460c89d" />

Finally, restart the Wazuh-agent to apply the change:

```bash
sudo systemctl restart wazuh-agent
```

