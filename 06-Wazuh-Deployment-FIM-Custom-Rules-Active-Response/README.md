# Wazuh Deployment in VirtualBox: FIM, Custom Rules, and Active Response

### Project Overview

This project outlines establishing and managing a **Wazuh** security system. Key objectives include configuring `File Integrity Monitoring (FIM)`, creating specialized threat detection rules, and implementing automated active response. This process occurs in a **VirtualBox** environment using `Ubuntu Server` and `Client` `VMs`.

### Goals

* Deploy and manage a **Wazuh** security information and event management (**SIEM**) solution.

* Configure `File Integrity Monitoring (FIM)` for threat detection.

* Define custom detection rules to enhance security.

* Implement automated active response mechanisms.

### Task Summary

* **Task 1:** Connect to the `Ubuntu Server VM` and review the **Wazuh** quickstart guide for installation details (`Ubuntu Server`).

* **Task 2:** Install the **Wazuh Manager** on the `Ubuntu Server`, save web access credentials, and update them with the server's `IP` (`Ubuntu Server`).

* **Task 3:** Access the **Wazuh** dashboard to confirm manager status and prepare the `Ubuntu Client VM` for agent deployment (`Ubuntu Server`).

* **Task 4:** Configure agent package settings, specifying the operating system and manager's address (`Ubuntu Server`).

* **Task 5:** Name the new agent and obtain installation commands for client setup (`Ubuntu Server`).

* **Task 6:** Deploy the **Wazuh** agent on the `Ubuntu Client`, activate its service, and verify its active status in the **Wazuh** dashboard (`Ubuntu Client & Server`).

* **Task 7:** Configure `File Integrity Monitoring (FIM)` on the `Ubuntu Client` by creating a monitored directory, adjusting the agent's configuration, and generating a test file for verification (`Ubuntu Client`).

* **Task 8:** Verify `FIM` operation by checking the **Wazuh** dashboard for file addition events (`Ubuntu Server`).

* **Task 9:** Define a custom data leakage detection rule on the `Ubuntu Client`, deploy it to the **Wazuh Manager**, and activate it by restarting the manager service (`Ubuntu Client & Server`).

* **Task 10:** Verify custom rule deployment and configure active response to trigger with custom rule (Ubuntu Server).

* **Task 11:** Review and configure active response within the custom rule file on the `Ubuntu Client`. Observe the triggered active response and dashboard updates on the `Ubuntu Server` (`Ubuntu Client & Server`).

### Step-by-Step Guide

---

### Task 1: Connect to the Ubuntu Server VM and review the Wazuh quickstart guide for installation details (Ubuntu Server)

![Figure 2 - Wazuh Quickstart Installation Guide](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure02_Wazuh_Quickstart_Installation_Guide.png)

* Established connection to `UbuntuServerHost` in **Oracle VirtualBox**.

* Navigated to **Wazuh’s** home page via web browser and reviewed the installation guide to obtain the command needed for the installation.

![Figure 3 - VirtualBox Manager Ubuntu Server Connection and Terminal Hostname](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure03_VirtualBox_Manager_Ubuntu_Server_Connection_and_Terminal_Hostname.png)

### Task 2: Install the Wazuh Manager on the Ubuntu Server, save web access credentials, and update them with the server's IP (Ubuntu Server)

![Figure 4 - Wazuh Installation & Credentials](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure04_Wazuh_Installation_Credentials.png)

* Executed `curl` command for the **Wazuh** installation on `UbuntuHost` (server).

* Installation provided web access credentials, which were saved to the desktop using `nano` to file `Wazuh_Information`.

* Identified `UbuntuHost` (server) `IP` using the `ip a` command (`10.0.2.23`) and updated the same file.

![Figure 5 - Server IP Verification & Credentials File](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure05_Server_IP_Verification_Credentials_File.png)

### Task 3: Access the Wazuh dashboard to confirm manager status and prepare the Ubuntu Client VM for agent deployment (Ubuntu Server)

![Figure 6 - Wazuh Dashboard No Agents Deployed](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure06_Wazuh_Dashboard_No_Agents_Deployed.png)

* Accessed **Wazuh** dashboard via web browser (`https://10.0.2.23:443`).

* Confirmed `0` registered agents, manager operational, web interface reachable.

* Returned to **Oracle VirtualBox Manager**, launched `UbuntuHost`.

* Confirmed `UbuntuServerHost` and `UbuntuHost` running.

![Figure 7 - VirtualBox Manager & Ubuntu Client Hostname](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure07_VirtualBox_Manager_Ubuntu_Client_Hostname.png)

### Task 4: Configure agent package settings, specifying the operating system and manager's address (Ubuntu Server)

![Figure 8 - Wazuh Deploy Agent Navigation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure08_Wazuh_Deploy_Agent_Navigation.png)

* Navigated to "Deploy new agent" in **Wazuh** dashboard.

* Selected `LINUX` with `DEB amd64`.

* Entered manager's `IP` (`10.0.2.23`) as "Server address."

![Figure 9 - Linux Agent Type & Server IP](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure09_Linux_Agent_Type_Server_IP.png)

### Task 5: Name the new agent and obtain installation commands for client setup (Ubuntu Server)

![Figure 10 - Assign Agent Name UbuntuClient](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure10_Assign_Agent_Name_UbuntuClient.png)

* Assigned agent name `UbuntuClient` and kept default agent group.

* **Wazuh** dashboard generated `wget`, `dpkg`, and `systemctl` commands, need to install the agent in the `UbuntuHost` (client).

![Figure 11 - Agent Installation Commands Generated](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure11_Agent_Installation_Commands_Generated.png)

### Task 6: Deploy the Wazuh agent on the Ubuntu Client, activate its service, and verify its active status in the Wazuh dashboard (Ubuntu Client & Server)

![Figure 12 - Wazuh Dashboard Active Agent](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure12_Wazuh_Dashboard_Active_Agent.png)

* Ran generated `wget` and `dpkg` command on `UbuntuHost` (client).

* Executed `systemctl` commands (`daemon-reload`, `enable`, `start`) to start the **Wazuh** agent service on the `UbuntuHost` (client).

* Returned to the `UbuntuHost` (server) and navigated to **Wazuh** dashboard's "Endpoints" page, confirming "`1 Active`" agent, `UbuntuClient` (`10.0.2.24`, `Ubuntu 24.04.2 LTS`, active status).

![Figure 13 - Agent Deployment & Service Activation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure13_Agent_Deployment_Service_Activation.png)

### Task 7: Configure File Integrity Monitoring (FIM) on the Ubuntu Client by creating a monitored directory, adjusting the agent's configuration, and generating a test file for verification (Ubuntu Client)

![Figure 14 - FIM Directory Creation & ossec.conf Config](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure14_FIM_Directory_Creation_ossec_conf_Config.png)

* Created `~/Desktop/UbuntuFIM` directory using the `mkdir` command.

* Modified **Wazuh** agent's main configuration file (`ossec.conf`) using `nano` to enable `syscheck` (`FIM`) to monitor the new directory with `check_all="yes"`, `realtime="yes"`, and `report_changes="yes"` attributes.

* Restarted **Wazuh** agent service.

* Created the file `testconfig.txt` using `gedit` to save the `FIM` configuration for a better visual record in the `UbuntuFIM` directory.

![Figure 15 - FIM testconfig.txt Creation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure15_FIM_testconfig_txt_Creation.png)

### Task 8: Verify FIM operation by checking the Wazuh dashboard for file addition events (Ubuntu Server)

![Figure 16 - Wazuh FIM Events Tab Navigation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure16_Wazuh_FIM_Events_Tab_Navigation.png)

* Navigated to the "Events" tab in `File Integrity Monitoring` for the `UbuntuClient` endpoint in the **Wazuh** dashboard.

* Confirmed "`File added to the system`" event for `/home/UbuntuHost/Desktop/UbuntuFIM/testconfig.txt`.

![Figure 17 - FIM File Added Event Confirmed](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure17_FIM_File_Added_Event_Confirmed.png)

### Task 9: Define a custom data leakage detection rule on the Ubuntu Client, deploy it to the Wazuh Manager, and activate it by restarting the manager service (Ubuntu Client & Server)

![Figure 18 - Customrule.txt Creation (Client)](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure18_Customrule_txt_Creation_Client.png)

* Created the file `customrule.txt` in the `UbuntuHost` (client) using `gedit` to save the custom rule configuration for a better visual in the `UbuntuFIM` directory.

* Edited `local_rules.xml` using `nano` on `UbuntuHost` (server) to deploy this rule, adding a rule with `id="100003"`, `level="12"`, to monitor the file `customrule.txt` for modified events, with the description 'A data leakage attempt has been detected. Rule configuration file named "customrule.txt" has been modified'.

* Restarted **Wazuh** manager service to activate the new rule.

![Figure 19 - local_rules.xml Edit & Rule Deployment (Server)](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure19_local_rules_xml_Edit_Rule_Deployment_Server.png)

### Task 10: Verify custom rule deployment and configure active response to trigger with custom rule (Ubuntu Server)

![Figure 20 - Wazuh Custom Rule Event Confirmed](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure20_Wazuh_Custom_Rule_Event_Confirmed.png)

* Navigated to the "Events" tab for `UbuntuClient` in the **Wazuh** dashboard, confirmed `customrule.txt` event.

* Modified `ossec.conf` file on `UbuntuHost` (server) using `sudo nano` to configure active response for the `UbuntuClient` agent with attributes `command="restart-wazuh"`, `location="defined-agent"`, `agent_id="001"`, and `rules_id="100003"`.

* Restarted **Wazuh** manager service to apply configurations.

![Figure 21 - ossec.conf Active Response Configuration](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure21_ossec_conf_Active_Response_Configuration.png)

### Task 11: Review and configure active response within the custom rule file on the Ubuntu Client. Observe the triggered active response and dashboard updates on the Ubuntu Server (Ubuntu Client & Server)

![Figure 22 - Wazuh Client Dashboard](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure22_Wazuh_Client_Dashboard.png)

* Opened `customrule.txt` on `UbuntuHost` (client) using `gedit`, to add the active response configuration (`command="restart-wazuh"`, `location="defined-agent"`, `agent_id="001"`, `rules_id="100003"`) for a better visual and trigger an active response event by modifying the file.

* Navigated to the `UbuntuClient` dashboard in `UbuntuHost` (server).
  
![Figure 23 - customrule.txt Active Response Config](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure23_customrule_txt_Active_Response_Config.png)

### Task 11: Continued.

![Figure 24 - Threat Hunting Events Navigation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure24_Threat_Hunting_Events_Navigation.png)

* Selected the "Threat Hunting" tab and then "Events" in the same section.

* The Events tab displayed the triggered active response by data leakage rule, confirming successful automation and effective alert generation.

![Figure 25 - Active Response Event Confirmed](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/06-Wazuh-Deployment-FIM-Custom-Rules-Active-Response/images/Figure25_Active_Response_Event_Confirmed.png)
