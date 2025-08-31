# Wazuh Deployment in VirtualBox: Windows 10 Agent & Log Management

### Project Overview

This project details setting up a **Wazuh** agent on a `Windows 10` virtual machine in **Oracle VirtualBox**. Key objectives include agent installation, configuring `File Integrity Monitoring (FIM)`, creating custom detection rules, and setting up active responses. It also covers log retention and deletion policies using `crontab` jobs and **Wazuh Indexer** management, aiming to demonstrate a complete security monitoring setup with endpoint detection and response.

### Goals

* Deploy and integrate a `Windows 10` **Wazuh** agent for endpoint monitoring within an **Oracle Virtual Box** environment.

* Implement advanced security monitoring features like `File Integrity Monitoring` and custom rule creation.

* Automate responses to security incidents within the **Wazuh** ecosystem.

* Manage log retention and deletion policies through both command-line and **Wazuh Indexer** interfaces.

### Task Summary

* **Task 1:** `Windows 10 VM` preparation and initial **Wazuh** dashboard overview.

* **Task 2:** Configuration of new agent deployment settings, including `Windows` packages and server address.

* **Task 3:** Setting optional agent details and reviewing generated `PowerShell` installation commands.

* **Task 4:** Execution of **Wazuh** agent installation, service start, and dashboard verification.

* **Task 5:** `FIM` directory creation, **Wazuh** agent configuration, and verification of setup.

* **Task 6:** Creation, confirmation, and placement of a `FIM` test file, then monitoring **Wazuh** dashboard for events.

* **Task 7:** Custom rule configuration on **Wazuh Manager’s** `local_rules.xml` and `Win10Client` documentation.

* **Task 8:** Event monitoring of `Win10Client` in **Wazuh** dashboard, configuring active response, and verifying implementation.

* **Task 9:** Triggering custom data leakage events on `Win10Client`, navigating `Threat Hunting` dashboard, and confirming active response.

* **Task 10:** `Crontab` configuration for automated yearly log retention and deletion on `UbuntuHost Server`.

* **Task 11:** Reviewed **Wazuh** dashboard and accessed `Index Management` policies for new policy creation.

* **Task 12:** Configured new **Wazuh Indexer Management** policy, defining `ID`, description, and `ISM` templates.

* **Task 13:** Defined and confirmed `ISM` policy states for log retention, including `'delete_alerts'` and `'initial'` states, actions, and transitions.

* **Task 14:** Reviewed and confirmed successful creation and active status of "`Wazuh-Yearly-Retention`" policy, observing states and transitions.

* **Task 15:** Navigated `Index Management`, applied "`Wazuh-Yearly-Retention`" policy to indices, and confirmed application status.

---

### Step-by-Step

### Task 1: Windows 10 VM preparation and initial Wazuh dashboard overview (Win10Client and UbuntuHost Server)

![Figure 1 - Wazuh Dashboard Active Ubuntu Agent](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure01_Wazuh_Dashboard_Active_Ubuntu_Agent.png)

* Booted the `Windows 10` virtual machine to prepare for the **Wazuh Agent** installation. (`Win10Client`)

* A command prompt was opened, observing the displayed `Windows` version. (`Win10Client`)

* The **Wazuh** endpoint management dashboard was viewed, noting the active `Ubuntu` agent and default group. (`UbuntuHost Server`)

![Figure 2 - Win10 VM Boot & CMD](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure02_Win10_VM_Boot_CMD.png)

### Task 2: Configuration of new agent deployment settings, including Windows packages and server address (UbuntuHost Server)

![Figure 3 - Deploy New Agent Windows MSI](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure03_Deploy_New_Agent_Windows_MSI.png)

* Selected the “Deploy new agent” option in the `Endpoint` dashboard.

* Packages for `Windows` (`MSI 32/64bits`) were selected.

* "Server address" was set with "`10.0.2.23`" for agent-server communication.

![Figure 4 - Server Address Configured](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure04_Server_Address_Configured.png)

### Task 3: Setting optional agent details and reviewing generated PowerShell installation commands (UbuntuHost Server)

![Figure 5 - Agent Details Win10Client Default Group](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure05_Agent_Details_Win10Client_Default_Group.png)

* Optional agent settings were configured, "`Win10Client`" as the agent name and selecting "`default`" as the group.

* The `PowerShell` command for downloading and installing the **Wazuh Windows agent** `MSI` was reviewed.

* The `NET START WazuhSvc` command was noted.

![Figure 6 - PowerShell Install Commands](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure06_PowerShell_Install_Commands.png)

### Task 4: Execution of Wazuh agent installation, service start, and dashboard verification (Win10Client and UbuntuHost Server)

![Figure 7 - Two Active Agents Dashboard](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure07_Two_Active_Agents_Dashboard.png)

* The **Wazuh agent** installation and service start commands were executed in `PowerShell`. (`Win10Client`)

* **Wazuh** service was started successfully. (`Win10Client`)

* `2` active agents (`1 Ubuntu`, `1 Windows`) were verified, and both agents were confirmed active in the **Wazuh** dashboard. (`UbuntuHost Server`)

![Figure 8 - Agent Install & Service Start](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure08_Agent_Install_Service_Start.png)

### Task 5: FIM directory creation, Wazuh agent configuration, and verification of setup (Win10Client)

![Figure 9 - Win10FIM Directory Creation & ossec.conf Edit](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure09_Win10FIM_Directory_Creation_ossec_conf_Edit.png)

* The "`Win10FIM`" directory was created on the desktop using the `New-Item` command in `PowerShell` and verified in `Windows File Explorer`.

* The **Wazuh agent’s** `ossec.conf` file was edited in `PowerShell` using `notepad`.

* A "`FIM Test Configuration`" section with `check_all="yes"` `realtime="yes"` `report_changes="yes"` for `C:\Users\Win10` was added.

* The **Wazuh** service was restarted in `PowerShell` to apply changes.

![Figure 10 - FIM Config & Service Restart](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure10_FIM_Config_Service_Restart.png)

### Task 6: Creation, confirmation, and placement of a FIM test file, then monitoring Wazuh dashboard for events (Win10Client and UbuntuHost Server)

![Figure 11 - FIM Event Confirmed](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure11_FIM_testconfig_txt_Event_Confirmed_in_Dashboard.png)

* The "`testconfig.txt`" file was created within "`Win10FIM`" using `New-Item` in `PowerShell` and opened with `notepad.exe`. (`Win10Client`)

* Saved `syscheck FIM` configuration for a better visual. (`Win10Client`)

* Navigated to the `File Integrity Monitoring` dashboard for the `Win10Client Endpoint` and then to the “`Events`” tab in the same menu. (`UbuntuHost Server`)

* Events for `c:/users/win10/desktop/win10fim/testconfig.txt` in the table were verified, confirming `FIM` functionality. (`UbuntuHost Server`)

![Figure 12 - testconfig.txt Creation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure12_testconfig_txt_Creation_in_FIM_Folder.png)

### Task 7: Custom rule configuration on Wazuh Manager’s local_rules.xml and Win10Client documentation (UbuntuHost Server and Win10Client)

![Figure 13 - local_rules.xml Custom Rule Add](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure13_local_rules_xml_Custom_Rule_Add.png)

* The `local_rules.xml` file for the **Wazuh Manager** was edited using `sudo nano` and a new custom rule was added for the `Win10Client` with the attributes `rule id="100004"` and `level="12"`. (`UbuntuHost Server`)

* The rule was configured to detect "`modified`" events for "`win10rules.txt`" with the description "`A data leakage attempt has been detected`". (`Win10Client`)

* The **Wazuh Manager** service was restarted to load rule changes. (`UbuntuHost Server`)

* The "`win10rules.txt`" file was created within "`Win10FIM`" using `New-Item` in `PowerShell` and opened with `notepad.exe`. The "`DataLeakage`” custom rule was saved for a better visual. (`Win10Client`)

![Figure 14 - Win10Client win10rules.txt Creation & Rule Save](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure14_Win10Client_win10rules_txt_Creation_Rule_Save.png)

### Task 8: Event monitoring of Win10Client in Wazuh dashboard, configuring active response, and verifying implementation (UbuntuHost Server)

![Figure 15 - Win10Client File Added Observed in Dashboard](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure15_Win10Client_File_Added_Observed_in_Dashboard.png)

* Updated "`Win10Client`" details were observed in the **Wazuh** dashboard and confirmed "`File added`" for `win10rules.txt`.

* The main **Wazuh** configuration file (`ossec.conf`) for the **Wazuh Manager** was edited using `sudo nano` and restarted to apply changes.

* Active response was configured in `ossec.conf` with attributes `restart-wazuh` for `Win10Client`.

![Figure 16 - Active Response Configured ossec.conf](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure16_Active_Response_Configured_ossec_conf.png)

### Task 9: Triggering custom data leakage events on Win10Client, navigating Threat Hunting dashboard, and confirming active response (Win10Client and UbuntuHost Server)

![Figure 17 - Data Leakage Event Confirmed](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure17_Data_Leakage_Event_Confirmed.png)

* The `win10rules.txt` file was modified to trigger the data leakage rule by adding the `Active Response` configuration for a better visual. (`Win10Client`)

* Navigated to the `Threat Hunting` dashboard of the `Win10Client Endpoint` and then to the “`Events`” tab of the same menu. (`UbuntuHost Server`)

* Verified the custom rule data leakage event for "`Win10Client`" was triggered in the **Wazuh** dashboard with the description "`A data leakage attempt has been detected. Rule configuration file named "customrule.txt" has been modified`". (`UbuntuHost Server`)

![Figure 18 - Trigger Data Leakage](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure18_Trigger_Data_Leakage.png)

### Task 10: Crontab configuration for automated yearly log retention and deletion on UbuntuHost Server (UbuntuHost Server)

![Figure 19 - Crontab Edit & Nano Selection](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure19_Crontab_Edit_Nano_Selection.png)

* Accessed the `crontab` file using `sudo crontab -e` in the "`UbuntuHost@server`" terminal to edit root's `crontab`. (`UbuntuHost Server`)

* `nano` was selected as the editor by inputting `1`. (`UbuntuHost Server`)

* Two tasks were defined in the `nano` editor for the `crontab` file: `0 18 * * * find /var/ossec/logs/alerts/ -type f -mtime +365` and `0 18 * * * find /var/ossec/logs/archives/ -type f -mtime +365` to configure yearly deletion/rotation of old **Wazuh** alerts and archives for the **Wazuh Manager**. (`UbuntuHost Server`)

* The "`Wazuh_Information`" text file was opened using `gedit` and added the `crontab` jobs for yearly deletion for a better visual. (`UbuntuHost Server`)

![Figure 20 - Crontab Jobs & Wazuh_Information Update](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure20_Crontab_Jobs_Wazuh_Information_Update.png)

### Task 11: Reviewed Wazuh dashboard and accessed Index Management policies for new policy creation (UbuntuHost Server)

![Figure 21 - Index Management Navigation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure21_Index_Management_Navigation.png)

* Navigated to the **Wazuh** dashboard's left navigation pane and selected “`Index Management`”. (`UbuntuHost Server`)

* The "`State management policies`" section in **Wazuh** "`Index Management`" was accessed, showing no existing policies. (`UbuntuHost Server`)

![Figure 22 - Index Management Policies Empty](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure22_Index_Management_Policies_Empty.png)

### Task 12: Configured new Wazuh Indexer Management policy, defining ID, description, and ISM templates (UbuntuHost Server)

![Figure 23 - Create Policy Visual Editor](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure23_Create_Policy_Visual_Editor.png)

* Selected the “`Create Policy`” option in `State management policies` and chose the "`Visual editor`" as the policy definition method in the "`Configuration method`" dialog. (`UbuntuHost Server`)

* "`Wazuh-Yearly-Retention`" was entered as the "`Policy ID`" and "`Policy to delete Wazuh alerts older than 365 days`" was entered as the "`Description`". (`UbuntuHost Server`)

* Selected "`Add template`" in the `ISM templates` section and set `Index patterns` to "`wazuh-alerts-*`" with "`Priority`" as "`1`". (`UbuntuHost Server`)

![Figure 24 - Policy ID Description ISM Template](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure24_Policy_ID_Description_ISM_Template.png)

### Task 13: Defined and confirmed ISM policy states for log retention, including 'delete_alerts' and 'initial' states, actions, and transitions (UbuntuHost Server)

![Figure 25 - Add State Delete Alerts](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure25_Add_State_Delete_Alerts.png)

* Selected the “`Add state`” option in the `States` section and entered “`delete_alerts`” as the state name. (`UbuntuHost Server`)

* The "`Delete`" action was added to the `delete_alerts` state interface using the “`Add action`” button. (`UbuntuHost Server`)

* Added a second state named `initial` with Its "`Order`" configured as "`Add before delete_alerts`". (`UbuntuHost Server`)

* Scrolled down to the `Transition` section of the `initial` state and added the "`Destination state`" as "`delete_alerts`". (`UbuntuHost Server`)

* "`Condition`" was set to "`Minimum Index Age`", with a value of "`365d`" and confirmed the addition with the “`Add transition`” button. (`UbuntuHost Server`)

![Figure 26 - Delete Alerts Initial State Transition Configured](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure26_Delete_Alerts_Initial_State_Transition_Configured.png)

### Task 14: Reviewed and confirmed successful creation and active status of "Wazuh-Yearly-Retention" policy, observing states and transitions (UbuntuHost Server)

![Figure 27 - Review States & Transitions](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure27_Review_States_Transitions.png)

* The "`States (2)`" section of the policy was reviewed, observing both `initial` and `delete_alerts` states. (`UbuntuHost Server`)

* It was confirmed that `initial` had "`Transitions: 1`" to "`delete_alerts`" with a "`Minimum index age is 365d`" trigger and `delete_alerts` had "`Actions: 1`" with a "`Delete`" action. (`UbuntuHost Server`)

* Selected the “`initial`” state as default using the dropdown and created the policy using the “`create`” button. (`UbuntuHost Server`)

* The creation and active status of the "`Wazuh-Yearly-Retention`" policy in the "`State management policies (1)`" table was confirmed. (`UbuntuHost Server`)

![Figure 28 - Policy Creation Confirmed & Active](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure28_Policy_Creation_Confirmed_Active.png)

### Task 15: Navigated Index Management, applied "Wazuh-Yearly-Retention" policy to indices, and confirmed application status (UbuntuHost Server)

![Figure 29 - Apply Policy to Indices](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure29_Apply_Policy_to_Indices.png)

* Navigated to the "`Indexes`" menu within the "`Index Management`" section and noted the existing `13` indexes. (`UbuntuHost Server`)

* Selected the “`Apply policy`” option from the "`Actions`" dropdown and chose "`Wazuh-Yearly-Retention`" policy from the following dropdown. (`UbuntuHost Server`)

* Policy application on the "`Indexes (13)`" table was confirmed, with a pop-up stating: "`Applied policy to 11 indices`." (`UbuntuHost Server`)

![Figure 30 - Policy Applied Confirmation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/07-Wazuh-Deployment-Windows-Agent-Log-Management/images/Figure30_Policy_Applied_Confirmation.png)
