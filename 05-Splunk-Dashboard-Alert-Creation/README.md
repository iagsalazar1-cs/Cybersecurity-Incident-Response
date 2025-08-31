# Splunk Dashboard and Alert Creation

### Project Overview

This project integrates two core **Splunk** exercises. First, interactive dashboards are created and modified for data display. Second, alerts for real-time event monitoring are configured. A central aim involves constructing a dashboard to observe `HTTP 404` errors from Apache web server logs. Following this, real-time alerts for these errors are established. Such a process demonstrates comprehensive data observation and analysis within **Splunk**.

### Goals

* Develop comprehensive skills in **Splunk** for data monitoring and analysis.

* Create and customize interactive **Splunk** dashboards to effectively visualize data, focusing on `HTTP 404` errors from Apache access logs.

* Configure and implement real-time **Splunk** alerts to proactively monitor specific events and thresholds, such as `HTTP 404` errors.

* Understand the end-to-end **Splunk** workflow, from data ingestion and visualization to proactive alerting.

### Task Summary

* **Task 1:** Initiate **Splunk**, Access Web Interface, and Generate `404` Traffic: Start the **Splunk** service via terminal, access its web interface via `Firefox`, and create `HTTP 404` status codes by refreshing a `URL`.

* **Task 2:** Navigate to Dashboards Section: Navigate from the **Splunk** web interface to "Search & Reporting," then to "Dashboards," skipping any tutorials.

* **Task 3:** Create a New Dashboard: Create a new **Classic Dashboard** titled "Apache Access Dashboards" with the description "404 Error."

* **Task 4:** Import Dashboard `XML`: Import dashboard `XML` by replacing the existing `XML` source in the edit screen with `Dashboard.xml` content, then save.

* **Task 5:** Customize Dashboard Panel: Customize an existing dashboard panel by duplicating it via `XML`, then update its title, `timechart` span, and earliest time setting.

* **Task 6:** Search for Apache Access Logs: Navigate to **Splunk's** Search & Reporting section and execute a query to locate Apache access logs.

* **Task 7:** Define Search for `404` Errors and Initiate Alert Creation: Refine a **Splunk** search to specifically find `404` errors in Apache logs, execute; then initiate the alert creation process from the search results.

* **Task 8:** Configure Alert Settings: Configure a new real-time alert with specific criteria, including title, trigger conditions based on result count, trigger actions, and throttling settings.

* **Task 9:** Trigger the Alert: Trigger the configured alert by generating sufficient `HTTP 404` errors through repeated page refreshes in `Firefox`.

* **Task 10:** Verify Triggered Alerts: Verify alert functionality by accessing **Splunk's** 'Triggered Alerts' section and reviewing the list.

### Step-by-Step Guide

---

### Task 1: Initiate Splunk, Access Web Interface, and Generate 404 Traffic

![Figure 2 - Splunk Web Login](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure02_Splunk_Web_Login.png)

* Initiated the **Splunk** service from a terminal.

* Navigated to the **Splunk** web interface and logged into the **Splunk** web interface at `localhost:8000` using `admin` credentials.

* Refreshed `http://localhost/random` in `Firefox` to generate `HTTP 404` status codes.

![Figure 3 - Splunk Service Initiated](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure03_Splunk_Service_Initiated.png)

![Figure 4 - Generate HTTP 404 Traffic](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure04_Generate_HTTP_404_Traffic.png)

### Task 2: Navigate to Dashboards Section

![Figure 5 - Navigate to Search and Reporting](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure05_Navigate_Search_and_Reporting.png)

* From the **Splunk** web interface, navigated to the "Search & Reporting" application.

* Went to the "Dashboards" section.

![Figure 6 - Navigate to Dashboards](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure06_Navigate_Dashboards.png)

### Task 3: Create a New Dashboard

![Figure 7 - Create Classic Dashboard Title](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure07_Create_Classic_Dashboard_Title.png)

* Configured and created a new **Classic Dashboard**.

* Set the title to "Apache Access Dashboards."

* Set the description to "404 Error."

![Figure 8 - Set Dashboard Description](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure08_Set_Dashboard_Description.png)

### Task 4: Import Dashboard XML

![Figure 9 - XML Code Entered in Splunk Dashboard](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure09_XML_Code_Entered_Splunk_Dashboard.png)

* In the dashboard's edit screen, replaced the existing `XML` source with content from `Dashboard.xml`.

* Saved the dashboard.

![Figure 10 - Opened Dashboard XML in Ubuntu Terminal](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure10_Opened_Dashboard_XML_Ubuntu_Terminal.png)

![Figure 11 - Original Dashboard.xml File](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure11_Original_Dashboard_XML_File.png)

### Task 5: Customize Dashboard Panel

![Figure 12 - Dashboard XML Duplication](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure12_Dashboard_XML_Duplication.png)

* Edited the dashboard's `XML` source and duplicated an existing panel.

* Modified the new panel's title to "`404` Status Code by Minutes."

* Set its `timechart` span to `1minute` and adjusted the earliest time value to `1h@h` and saved the changes.

![Figure 13 - Customized Dashboard Panel View](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure13_Customized_Dashboard_Panel_View.png)

### Task 6: Search for Apache Access Logs

![Figure 14 - Select Search & Reporting in Splunk](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure14_Select_Search_and_Reporting.png)

* From the left navigation pane in **Splunk**, selected "Search & Reporting."

* Entered `source="/var/log/apache2/access.log"` in the search bar and clicked the search icon.

![Figure 15 - Apache Access Logs Search Query](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure15_Apache_Access_Logs_Search_Query.png)

### Task 7: Define Search for 404 Errors and Initiate Alert Creation

![Figure 16 - Refine Search Query for 404 Errors](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure16_Refine_Search_Query_404_Errors.png)

* Refined the search query to `source="/var/log/apache2/access.log" AND status_code=404` and executed the search.

* Clicked "Save As" and selected "Alert" to begin alert creation.

![Figure 17 - Initiate Alert Creation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure17_Initiate_Alert_Creation.png)

### Task 8: Configure Alert Settings

![Figure 18 - Configure Alert Settings (Title, Type, Trigger Conditions)](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure18_Configure_Alert_Settings_Part1.png)

* Configured the new alert with the title "`Dir.Enum`."

* Set the alert type to "Real-time."

* Defined the trigger condition as "Number of Results" being "greater than: 4" "For each result."

* Added "Add to Triggered Alerts" as an action.

* Throttled by `ip_addr_count` and saved the alert.

![Figure 19 - Configure Alert Settings (Trigger Actions, Throttling)](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure19_Configure_Alert_Settings_Part2.png)

### Task 9: Trigger the Alert

![Figure 20 - Splunk Generated Alert for 404 Errors](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure20_Splunk_Generated_Alert_404_Errors.png)

* Opened a new `Firefox` tab.

* Navigated to `http://localhost/random`.

* Refreshed the page `5` times to generate sufficient `404` errors to trigger the alert.

![Figure 21 - Firefox 404 Error Generation](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure21_Firefox_404_Error_Generation.png)

### Task 10: Verify Triggered Alerts

![Figure 22 - Splunk Dir Enum Dashboard (Navigated to Triggered Alerts Menu)](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure22_Splunk_Dir_Enum_Dashboard_Navigated_Triggered_Alerts.png)

* Returned to the **Splunk** web application tab.

* Clicked "Activity > Triggered Alerts" from the top right menu.

* Reviewed the list to confirm the alert has triggered.

![Figure 23 - Triggered Alerts Menu in Splunk](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/05-Splunk-Dashboard-Alert-Creation/images/Figure23_Triggered_Alerts_Menu_Splunk.png)
