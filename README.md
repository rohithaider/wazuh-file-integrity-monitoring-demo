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

# Wazuh Configuration for File Integrity Monitoring in Ubuntu.
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
## Testing the configuration
I created a text file inside the monitored directory and waited for about 5 seconds to give Wazuh time to detect the creation event. After that, I opened the file, added some content to it, and saved the changes. Again, I waited for another 5 seconds to allow the file modification to be picked up. Finally, I deleted the text file from the monitored directory and waited a few more seconds so the deletion event could be captured by the File Integrity Monitoring system.

1. Creating the file
```bash
sudo touch /root/testfile.txt
sleep 5
```
2. Modify the file
```bash
echo "This is a test modification" | sudo tee -a /root/testfile.txt
sleep 5
```
3. Delete the file
```bash
sudo rm /root/testfile.txt
sleep 5
```

<img width="571" height="410" alt="image" src="https://github.com/user-attachments/assets/1669febb-95ab-40a4-a187-0d814dba4372" />

Now let's go the Wazuh dashboard and open the Ubuntu agent. From there go to the File Integrity Monitoring.

<img width="1213" height="554" alt="image" src="https://github.com/user-attachments/assets/4d20e839-ef8b-4e3a-8e29-50a38eeb5d1f" />

Here you can see that the events are showing the in the file integrity monitoring. 
<img width="1213" height="554" alt="image" src="https://github.com/user-attachments/assets/004271dc-054d-48e4-9629-eac156130859" />



