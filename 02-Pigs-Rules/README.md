# Pigs Rules - TDX Arena Certification Report

### Executive Summary

This project outlines leveraging network traffic analysis and Snort IDS rule development to identify and alert on malicious activity within a simulated network. The primary objective is to detect and respond to threats by utilizing `tcpdump` for passive network traffic analysis on a specified interface. This is followed by deep packet inspection on captured data to pinpoint suspicious network flows for malicious traffic identification. A custom Snort rule is then developed and deployed to identify and generate alerts for the identified malicious traffic. Finally, the implemented Snort rule is verified through console output and the SIEM interface (`Snorby`) for alert validation, retrieving the flag.

### Findings and Analysis

| Finding | Finding Details | Description |
| :--- | :--- | :--- |
| **File** | `syn_capture.txt` | File containing port scan activity. `tcpdump` packet capture file showing port scanning activity, identifying the threat actor scanning `172.29.0.3`. |
| **IP Address** | `172.29.0.1` | Suspicious IP Address. IP address of the threat actor identified after packet analysis. |
| **Hash** | MD5 `1fdcf70d937c1d1796a534fdb9e79c` | Flag required to complete the challenge, displayed in the `Snorby` GUI (Graphical User Interface) after configuring the Snort rule properly. |

### Methodology

#### Tools and Technologies Used

* `tcpdump`: Command-line packet analyzer used for capturing and analyzing network traffic.
* `nano`: Command-line text editor used for modifying configuration and rule files.
* **Snort:** Open-source network intrusion detection system (IDS) used for real-time traffic analysis and packet logging.
* **Snorby:** Web-based application that provides a graphical user interface for analyzing Snort alerts.

#### Investigation Process

1.  **Interface Identification:** Identified `eth0` as the primary network interface for Snort monitoring, with IP address `172.17.0.86`.
   
    ![Figure 1 - Interface Identification](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure01_Interface_Identification.png)

2.  **Initial Traffic Capture:** Captured general network traffic using the tool `tcpdump` and saved it onto a file for analysis.
   
    ![Figure 2 - Initial Traffic Capture](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure02_Initial_Traffic_Capture.png)

3.  **Initial Traffic Analysis:** Analysis of the text file `capture.txt` revealed several network TCP SYN packets originating from IP `172.29.0.1` targeting ports on host `172.29.0.3`.
   
    ![Figure 3 - capture.txt Analysis](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure03_capture_txt_Analysis.png)

4.  **SYN Packet Traffic Capture:** Captured only SYN packet network traffic using a different command variation with `tcpdump` and saved it onto a file for analysis.
   
    ![Figure 4 - SYN Packet Traffic Capture](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure04_SYN_Packet_Traffic_Capture.png)

5.  **Port Scan Confirmation:** Analyzed the contents of the `syn_capture.txt` file and observed a pattern suggesting port scanning activity, identifying `172.29.0.1` as the threat actor performing the scan on `172.29.0.3`.
   
    ![Figure 5 - syn_capture.txt Analysis](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure05_syn_capture_txt_Analysis.png)

6.  **Custom Snort Rule Creation:** Located the local Snort rules file and added a custom Snort rule to detect the port scan, using the `nano` command.
   
    ![Figure 6 - nano Editor and Snort Configuration](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure06_nano_Editor_and_Snort_Configuration.png)
    
    ![Figure 7 - Port Scanning Rule](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure07_Port_Scanning_Rule.png)

7.  **Test Rule in Console Mode:** Tested rule by running Snort in console mode, which successfully displayed alerts for the detected port scan.
   
    ![Figure 8 - Snort Console Mode](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure08_Snort_Console_Mode.png)
    
    ![Figure 9 - Console Port Scan Alerts](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure09_Console_Port_Scan_Alerts.png)

8.  **Run Snort in IDS Mode for Snorby:** Snort was run in IDS mode to log alerts into `Snorby` (Snort Web Graphical Interface).
   
    ![Figure 10 - Snort Alert Redirection to Snorby](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure10_Snort_Alert_Redirection_to_Snorby.png)

9.  **Flag Retrieval from Snorby:** Accessed the `Snorby` web interface dashboard and noted a high number of Snort Alert events, confirming the rule was effectively implemented. The challenge flag was successfully retrieved from the `Snorby` GUI.
    
    ![Figure 11 - Flag Retrieval from Snorby](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/02-Pigs-Rules/images/Figure11_Flag_Retrieval_from_Snorby.png)

### Recommendations

1.  **Immediate Blocking:** Implement firewall rules to immediately block all incoming traffic from the source IP address `172.29.0.1` to `172.29.0.3`.
2.  **Source Investigation:** Conduct a thorough investigation into the source `172.29.0.1`. Determine if this IP address belongs to an internal asset or an external entity.
3.  **Target Assessment:** Perform a vulnerability assessment on `172.29.0.3` to identify any open ports or services that were targeted by the scan. Review logs on `172.29.0.3` for any signs of successful connections or follow-up activity after the scan.

### Appendix A: Detailed Findings

#### File-related Findings

| File | Path | Contents |
| :--- | :--- | :--- |
| `capture.txt` | `/home/snort/Desktop/capture.txt` | General network traffic captured using `tcpdump` that revealed repeated TCP SYN packets originating from IP `172.29.0.1` targeting several ports on host `172.29.0.3`. |
| `syn_capture.txt` | `/home/snort/Desktop/syn_capture.txt` | SYN packet network traffic captured using a `tcpdump` command variation that revealed a pattern suggesting port scanning activity, identifying `172.29.0.1` as the threat actor scanning `172.29.0.3`. |

#### Endpoint-related Findings

| Host/Port | Description |
| :--- | :--- |
| `Ip-172-29-0-1.ec2.internal` on port `36730` identified as the threat actor. | Numerous ports on the victim machine were scanned by the malicious IP `172.29.0.1` (e.g., `46667`, `44676`, `23883`, etc.). |

#### Network-related Findings

| Type | Details |
| :--- | :--- |
| URL/API | `https://pigs-rule-snorby` - `Snorby` Dashboard for analyzing Snort alerts. |
| Process | After successfully configuring the custom Snort rule, the challenge flag was retrieved from the `Snorby` Graphical Interface. |
| MD5 Hash Flag | `1fdcf70d937c1d1796a534fdb9e79c` |

### Appendix B: Command History

| Syntax | Description |
| :--- | :--- |
| `ifconfig` | Displays details about all active network interfaces, such as their IP addresses. |
| `sudo tcpdump -i eth0 -c 1000 > capture.txt` | Captures the first 1000 network packets (`-c 1000`) on the `eth0` interface (`-i`) and saves the output to a file named `capture.txt`. |
| `sudo tcpdump -i eth0 -n -c 1000 "tcp[tcpflags] & tcp-syn != 0" > syn_capture.txt` | Captures the first 1000 TCP packets on the `eth0` interface that have the SYN flag set (indicating the start of a TCP connection attempt) and saves the packet data to the `syn_capture.txt` file, without performing hostname or service name lookups (`-n`). |
| `ls /etc/snort/rules/local.rules` | Checks if the `local.rules` file exists before adding or modifying custom Snort rules within it. |
| `nano /etc/snort/rules/local.rules` | Command-line based text editor used to view and edit the custom Snort rules in the `local.rules` file. |
| `tcp 172.29.0.1 any -> 172.29.0.3 any (flags:S; msg:"Potential Port Scan from 172.29.0.1 to 172.29.0.3"; sid:1000001; rev:1;)` | Generates an alert where Snort is configured whenever it detects a TCP SYN packet from IP address `172.29.0.1` going to any port on IP address `172.29.0.3`. The alert message will indicate a "Potential Port Scan" and identified by SID `1000001` and is currently at revision `1`. |
| `sudo snort -A console -c /etc/snort/snort.conf -i eth0` | Used to start the Snort Intrusion Detection System (IDS) in alert console mode from its configuration file to monitor the network traffic on the `eth0` interface, and displays any generated alerts directly in the console. |
| `sudo snort -c /etc/snort/snort.conf` | Used to start the Snort Intrusion Detection System (IDS) using its configuration file and monitor alerts, which will show on the Snort web interface (`Snorby`). |
