# Mail n’ Trail - TDX Arena Certification Report

### Executive Summary

This document outlines the process and findings of the "Mail n' Trail" CTF challenge. The primary objective of this challenge is to investigate suspicious download activities identified by a honeypot and subsequently access and analyze a Splunk system. This involves connecting to a mail server to retrieve critical emails, using the obtained credentials to log into Splunk, searching for and identifying suspicious activities within the Splunk logs, accessing the source of these activities, and finally decoding a message to uncover the flag. The challenge provides initial mail server credentials and instructions to calculate the server IP based on the user's IP address.

### Findings and Analysis

| Finding | Finding Details | Description |
| :--- | :--- | :--- |
| **Credentials** | Splunk Server Credentials Username: `admin` Password: `CTF_Final!` | Found by connecting to the mail server via `POP3` and retrieving message 4. |
| **URL** | `https://pastebin.com/raw/0cs1NHvh` | URL that contained the encoded flag. Found by analyzing **Cowrie** honeypot logs in **Splunk**, which revealed `curl` and `wget` commands downloading content from various `Pastebin` URLs. |
| **Encoded String** | Base64 Sequence `Q29uZ3JhdHMsIHlvdSBoYXZlIGZpbmlzaGVkIENJVF9GSU5US0BTCBZDUVNjZXNzZnVsbHk=` | Found by navigating to the Pastebin URL (`https://pastebin.com/raw/0cs1NHvh`) in a web browser. |
| **Decoded Message Flag** | Congrats, you have finished CIT_FINAL successfully | Found by using **CyberChef's** "From Base64" recipe to decode the extracted encoded string. |

### Methodology

#### Tools and Technologies Used

* **Command Prompt (`cmd.exe`):** Used for network commands like `ipconfig` and `telnet`.
* **Windows Control Panel / Server Manager:** Used to enable the `Telnet Client` feature.
* **Web Browser (Google Chrome):** Used to access the **Splunk** web interface and `Pastebin` URLs.
* **Splunk:** The **Security Information and Event Management (SIEM)** platform used for log analysis and identifying suspicious activities.
* **CyberChef:** An online tool used for decoding the final string to reveal the flag.

#### Investigation Process

1.  **Identify Windows Host IP and Calculate Mail Server IP:** Entered the `ipconfig` command to identify the Window's host IP address `10.0.220.220` and calculated `10.0.220.221` as the Mail Server's address (Windows Host IP +1 as per the challenge instructions).

    ![Figure 1 - Identify Windows Host IP](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure01_Identify_Windows_Host_IP.png)

2.  **Check Mail Services using Telnet:** Telnet check for `IMAP` (143), `POP3` (110), and `SMTP` (25) mail services on `10.0.220.221` failed. The `telnet` command was not recognized, indicating `Telnet Client` was not enabled on the Windows host.

    ![Figure 2 - Check Mail Services using Telnet](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure02_Check_Mail_Services_Telnet.png)

3.  **Navigate to Control Panel and Server Manager:** Navigated to Control Panel, clicked on "Programs", then "Turn Windows features on or off", and was redirected to the Server Manager.

    ![Figure 3 - Navigate to Control Panel and Server Manager](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure03_Navigate_Control_Panel_Server_Manager.png)

4.  **Install Telnet Client with Wizard in the Server Manager:** In the Server Manager, proceeded through the "Add Roles and Features Wizard," selecting the only available server, until the "Features" section was reached. Ticked the box for "`Telnet Client`" and clicked "Next" to install the service.

    ![Figure 4 - Server Manager’s Add Roles and Features](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure04_Server_Manager_Add_Roles_Features.png)

    ![Figure 5 - Telnet Client in Features Section](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure05_Telnet_Client_Features_Section.png)

5.  **Attempt to Connect to IMAP After Installing Telnet:** After installing the `Telnet Client`, re-attempted to connect to the mail server with the command `telnet 10.0.220.221 143` (`IMAP`), but the connection to port `143` failed with "Connect failed".

    ![Figure 6 - Attempt to Connect to IMAP After Installing Telnet](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure06_Connect_IMAP_After_Telnet.png)

6.  **Successfully Connected to POP3 and Logged In with Credentials:** Attempted to connect to mail server with the command `telnet 10.0.220.221 110` (`POP3`). The connection was successful, receiving the banner `+OK Dovecot (Ubuntu) ready`. Logged in using `USER johnd` and `PASS toor` commands and credentials.

    ![Figure 7 - Attempt to connect to mail server with the command telnet 10.0.220.221 110 (POP3)](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure07_Connect_POP3_Telnet.png)`

    ![Figure 8 - Logged in using USER johnd and PASS toor commands and credentials](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure08_LoggedIn_POP3_Credentials.png)

7.  **Checked Mailbox Status and Listed Messages:** Used the `STAT` command to get mailbox statistics and the `LIST` command to list messages. Received `+OK 4 messages:` followed by the message numbers and their sizes.

    ![Figure 9 - Checked Mailbox Status and Listed Messages](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure09_Mailbox_Status_Listed_Messages.png)

8.  **Retrieved Messages and Found Splunk Credentials:** Used the `RETR` command to retrieve all messages. Messages 1 thru 3 did not contain the Splunk credentials. Used the `RETR 4` command to retrieve message 4. It contained the Splunk URI (`http://[SERVER-IP]:8000/`) and credentials (`username: admin`, `password: CTF_Final!`).

    ![Figure 10 - Retrieved Message 4 and Splunk Credentials](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure10_Retrieved_Message_4_Splunk_Credentials.png)

9.  **Access and Log In to Splunk:** Used the web browser to navigate to the Splunk server at `http://10.0.220.221:8000`. The Splunk login page loaded successfully. Entered the retrieved credentials (`username: admin`, `password: CTF_Final!`). Successfully logged into the Splunk system.

    ![Figure 11 - Entered the retrieved credentials](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure11_Entered_Retrieved_Credentials.png)

    ![Figure 12 - Successfully logged into the Splunk system](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure12_Logged_Into_Splunk.png)

10. **Navigate to Search & Reporting and Search for Cowrie Events:** From the Splunk Home page, navigated to the "Search & Reporting" application. Entered the honeypot's name, `cowrie`, into the search bar and changed the time frame to "All time" in the dropdown. The search populated `149,353` log events for the **Cowrie** honeypot.

    ![Figure 13 - Navigate to Search & Reporting in Splunk](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure13_Navigate_Search_Reporting_Splunk.png)

    ![Figure 14 - Cowrie’s Event Search](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure14_Cowrie_Event_Search.png)

11. **Identify Suspicious Download Activities in Splunk:** Narrowed the search down by filtering for common download commands: `cowrie (wget OR curl OR ftp OR scp OR sftp OR tftp)`. The search successfully populated `28` events that indicated potential download activity. These events clearly showed `curl` and `wget` commands being executed, targeting `pastebin.com/raw/` URLs.

    ![Figure 15 - filtering for common download commands](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure15_Filtering_Download_Commands.png)

    ![Figure 16 - Pastebin.com/raw/ URLs](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure16_Pastebin_Raw_URLs.png)

12. **Analyze Extracted URL: jpSBiHjC:** Navigated to `https://pastebin.com/raw/jpSBiHjC`. The page displayed "Connecting: The site rejected your request, try again later."

    ![Figure 17 - Analyze Extracted URL: jpSBiHjC](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure17_Analyze_URL_jpSBiHjC.png)

13. **Analyze Extracted URL: 2QAuwmHe:** Navigated to `https://pastebin.com/raw/2QAuwmHe`. The page displayed "Downloading File ===========>> script.bat", indicating this URL contained a relevant file.

    ![Figure 18 - Analyze Extracted URL: 2QAuwmHe](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure18_Analyze_URL_2QAuwmHe.png)

14. **Analyze Extracted URL: ZTWf2ZmP:** Navigated to `https://pastebin.com/raw/ZTWf2ZmP`. The page contained the message "No, it's not it.", indicating this URL did not hold the flag or next decoding step.

    ![Figure 19 - Analyze Extracted URL: ZTWf2ZmP](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure19_Analyze_URL_ZTWf2ZmP.png)

15. **Analyze Extracted URL: 0cs1NHvh:** Navigated to `https://pastebin.com/raw/0cs1NHvh`. The page displayed a long string of characters, which were the encoded message for the flag.

    ![Figure 20 - Analyze Extracted URL: 0cs1NHvh](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure20_Analyze_URL_0cs1NHvh.png)

16. **Decode the Flag:** Navigated to the **CyberChef** website to analyze the string of characters. **CyberChef's** "Magic" operation identified the string as a Base64 encoded series of characters, or potentially Base65. Used the "From Base64" recipe in **CyberChef** to decode the string, obtaining the message: "Congrats, you have finished CIT_FINAL successfully". This was the flag for the challenge.

    ![Figure 21 - CyberChef's "Magic" Operation String Identification](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure21_CyberChef_Magic_Operation.png)

    ![Figure 22 - CyberChef's "From Base64" Operation String Decoding and Flag](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/04-Mail-n-Trail/images/Figure22_CyberChef_Base64_Decoding_Flag.png)

### Recommendations

1.  **Implement Stronger Honeypot Monitoring:** Enhance real-time alerting and automated analysis for honeypot logs to quickly identify and respond to suspicious activities, especially unusual download patterns.
2.  **Review Mail Server Security:** Ensure that mail server configurations follow best practices, including strong password policies and multi-factor authentication, to prevent unauthorized access.
3.  **Regularly Update and Patch Systems:** Keep all systems, including **Splunk** and any honeypots, updated with the latest security patches to mitigate known vulnerabilities.
4.  **Automate Threat Intelligence Feeds:** Integrate threat intelligence feeds with **Splunk** to automatically flag known malicious URLs or IP addresses observed in logs.

### Appendix A: Detailed Findings

#### Credential-related Findings

| Item | Details | Description |
| :--- | :--- | :--- |
| **Username** | `admin` | Splunk Server credentials found by connecting to the mail server via `POP3` and retrieving message 4. |
| **Password** | `CTF_Final!` | Splunk Server credentials found by connecting to the mail server via `POP3` and retrieving message 4. |

#### Network-related Findings

| Item | Details | Description |
| :--- | :--- | :--- |
| **URL** | `https://pastebin.com/raw/0cs1NHvh` | Found by analyzing **Cowrie** honeypot logs in **Splunk**, which revealed `curl` and `wget` commands downloaded content from various `Pastebin` URLs. |
| **CMD Command** | `curl` | Found by analyzing **Cowrie** honeypot logs in **Splunk**, which revealed `curl` and `wget` commands downloaded content from various `Pastebin` URLs. |

#### Forensic-related Findings

| Item | Details | Description |
| :--- | :--- | :--- |
| **Encoded String** | `Q29uZ3JhdHMsIHlvdSBoYXZlIGZpbmlzaGVkIENJVF9GSU5US0BTCBZDUVNjZXNzZnVsbHk=` | Found by navigating to the Pastebin URL (`https://pastebin.com/raw/0cs1NHvh`) in a web browser. |
| **Sequence Type** | Base64 | Found by navigating to the Pastebin URL (`https://pastebin.com/raw/0cs1NHvh`) in a web browser. |
| **Decoded Message** | Congrats, you have finished CIT_FINAL successfully | Found by using **CyberChef's** "From Base64" recipe to decode the extracted encoded string, which served as the challenge flag. |

### Appendix B: Other Extracted URL Links from Splunk

| URL | CMD Command | Description |
| :--- | :--- | :--- |
| `https://pastebin.com/raw/jpSBiHjC` | `curl` | Navigated to the URL using a web browser. The page displayed "Connecting: The site rejected your request, tried again later." |
| `https://pastebin.com/raw/2QAuwmHe` | `curl` | Navigated to the URL using a web browser. The page displayed "Downloading File ===========>> script.bat", indicating this URL contained a relevant file. |
| `https://pastebin.com/raw/ZTWf2ZmP` | `wget` | Navigated to the URL using a web browser. The page contained the message "No, it's not it.", indicating this URL did not hold the flag or next decoding step. |
