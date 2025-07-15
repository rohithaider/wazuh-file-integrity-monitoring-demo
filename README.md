# Wazuh File Integrity Monitoring (FIM) - Demonstration

This repository contains configuration files, sample logs, and step-by-step procedures to demonstrate File Integrity Monitoring (FIM) using Wazuh. It shows how Wazuh detects and logs file modifications, creations, and deletions on monitored endpoints in real time.

## Objective

To provide a clear and repeatable demonstration of Wazuh’s FIM capability by file modification and observing alert generation within the Wazuh dashboard.

---

## Environment Setup
- **Wazuh Manager**: Hosted on a cloud server (e.g., Linode, AWS). I hosted my cloud server in Linode.
  A documentation for deploying Wazuh Manager can be found here: [Wazuh Docker SIEM Setup](https://github.com/rohithaider/wazuh-docker-siem-setup)
- **Wazuh Agent**: Installed on Ubuntu VM & Windows 10 (VirtualBox)
- **Version**: Wazuh 4.12+
- **Log Collection**: Direct agent-to-manager communication ```(no Filebeat)```

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

<img width="567" height="378" alt="image" src="https://github.com/user-attachments/assets/33577612-ebc5-46b6-bf27-537ba41e862b" />


Finally, restart the Wazuh-agent to apply the change:

```bash
sudo systemctl restart wazuh-agent
```
## Testing the configuration in Ubuntu 
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

<img width="1207" height="545" alt="image" src="https://github.com/user-attachments/assets/1bab6081-6af8-4df0-9cdf-3faaeff1c7cc" />



Here you can see that the events are showing the in the file integrity monitoring. 

<img width="1211" height="547" alt="image" src="https://github.com/user-attachments/assets/6d23f1ec-629d-42be-b70f-39fbc25c27b2" />


# Wazuh Configuration for File Integrity Monitoring in Windows.

First edit the ossec.conf in administrative mode. ```C:\Program Files (x86)\ossec-agent\ossec.conf```.

<img width="997" height="645" alt="image" src="https://github.com/user-attachments/assets/ded998fe-6773-4a3a-b4f7-53f374109e74" />

, and add this block 

```bash
<directories check_all="yes" report_changes="yes" whodata="yes" realtime="yes">C:\Users\<USER_NAME>\Desktop</directories>
```

#note: I added ```whodata="yes"``` to see who modifies data. 

#WARNING: CHANGE YOUR USERNAME ABOVE!

<img width="980" height="632" alt="image" src="https://github.com/user-attachments/assets/af841d2f-3cd1-4a60-aa44-68c7024f2237" />


Finally, change the ossec.conf file and restart Wazuh agent in windows 10. 



## Testing the configuration in Windows 10. 

<img width="1332" height="633" alt="image" src="https://github.com/user-attachments/assets/acabf93f-bca5-4774-9a9e-3ba98c3bb678" />

You can now see that the Wazuh File Integrity monitoring system is actively reporting the alerts about file system changes in Windows 10.


---

## Resources

- [File Integrity Monitoring - Proof of Concept Guide](https://documentation.wazuh.com/current/proof-of-concept-guide/poc-file-integrity-monitoring.html) - Official Wazuh documentation for setting up and testing file integrity monitoring.



# Thank you.













