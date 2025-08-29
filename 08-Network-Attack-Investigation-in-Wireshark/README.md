# Network Attack Investigation in Wireshark

### Overview

This project involves analyzing network traffic captured in `.pcap` files using **Wireshark** to investigate simulated network attacks. As a network security analyst, the goal is to identify the attacker, the timing of the attack, the targeted machine, and the techniques employed. This will be done by filtering and examining network packets to uncover critical details about the security incident. The investigation concludes with the finding's details for both `.pcap` files being summarized at the end report.

### Goals

* Identify the `IP address` of the attacker.

* Determine the exact time the attack took place.

* Locate the `IP address` of the attacked machine.

* Identify the `TCP ports` the attacker attempted to access and which ports are open.

* Determine which of a given list of websites were not visited by legitimate users.

* Write a summary of the investigation's findings

### Tasks

* **Task 1:** Identify Attacker's `IP Address`: Analyze network traffic from `.pcap` file `1` to identify the attacker's `IP address` (`172.31.55.198`, `172.31.43.23`, `172.31.55.149`, or `93.184.221.240`).

* **Task 2:** Determine Attack Time and Locate Attacked Machine's `IP Address`: Determine the attack time and locate the attacked machine's `IP address`.

* **Task 3:** Identify Attacked Ports: Examine the traffic to determine which ports the attacker attempted to access.

* **Task 4:** Investigate `TCP Handshake`: Investigate the `TCP` three-way handshake used to find open ports, focusing on ports like `445`, `135`, and `53`.

* **Task 5:** Identify the second Attacker's `IP Address` from `.pcap` file `2`: Analyze network traffic to identify the address (`172.31.110.51`, `172.31.43.23`, `172.31.100.112`, or `93.184.221.240`).

* **Task 6:** Determine Attack Time: Determine the exact time of the attack.

* **Task 7:** Investigate `TCP Handshake`: Investigate the attacker's `TCP` three-way handshake requests.

* **Task 8:** Locate Attacked Machine's `IP Address`: Locate the attacked machine's `IP address`.

* **Task 9:** Determine Open Ports: Determine which ports are open on the attacked machine: `110`, `445`, `21`, and `3306`.

* **Task 10:** Identify Unvisited Websites: Determine which websites were `NOT` visited by legitimate users: `www.msn.com`, `www.yahoo.com`, `www.bing.com`, and `www.google.com`.

*  **Conclusion of the Investigation:** The investigation of two `.pcap` files revealed two distinct network attack incidents, indicative of network mapping. See conclusion section for a full summary of the investigation's findings.

### Task 1: Identify Attacker's IP Address: Analyze network traffic from .pcap file 1 to identify the attacker's IP address (172.31.55.198, 172.31.43.23, 172.31.55.149, or 93.184.221.240).

![Figure 1 - PCAP File 1 IP Address Identification](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure01_PCAP_File_IP_Address_Identification.png)

1. The scenario involves an organization that was the victim of two network attacks, and the traffic from these incidents was captured in two `.pcap` files. The task is to investigate the attacks using **Wireshark** and gather critical information about the incident. Observed that the `.pcap` file `1` has `13,927` captured packets.

2. Investigated the following `IP` addresses using the filter `ip.addr==`: `172.31.55.198`, `172.31.43.23`, `93.184.221.240`. Noticed that all three `IP` addresses are present in various packets and do not appear to exhibit suspicious behavior.

3. Filtered the `IP address 172.31.55.149`. Notice that `172.31.55.149` exhibits suspicious behavior with many `TCP` connections and `RST` responses, indicating that this address belongs to the attacker.

![Figure 2 - Attacker IP Filtered Traffic](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure02_Attacker_IP_Filtered_Traffic.png)

### Task 2: Determine Attack Time and Locate Attacked Machine's IP Address: Determine the attack time and locate the attacked machine's IP address.

![Figure 3 - Time of Day Display Format](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure03_Time_of_Day_Display_Format.png)

1. With the packets filtered using the attacker's `IP address 172.31.55.149`, changed the time display format to `Time of Day` by navigating to `View > Time Display Format > Time of Day`.

2. Observed that the time the attack began is `06:05:01`. Noticed that the attacker repeatedly tries to connect to the `IP address 172.31.55.198`, indicating that this was the victim's `IP address`.

![Figure 4 - Attack Start Time and Victim IP](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure04_Attack_Start_Time_and_Victim_IP.png)

### Task 3: Identify Attacked Ports: Examine the traffic to determine which ports the attacker attempted to access.

![Figure 5 - Attacked Ports](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure05_Attacked_Ports.png)

1. Noticed that the attacker sent multiple requests to the victim machine via various `TCP ports`, including `21`, `443`, and `445`. To determine how many packets involving the attacker `IP address 172.31.55.149` used `TCP Port 443`, added the `tcp.port == 443` filter to the filter bar to display the packets involving the attacker.

2. Ran the filter: `ip.addr == 172.31.55.149 && tcp.port == 443`.

3. Noticed that the attacker is the source and the victim is the destination in `10` packets to `Port 443` from various port numbers.

![Figure 6 - Attacker IP Address and Port 443 Filtered Packets](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure06_Attacker_IP_Address_and_Port_443_Filtered_Packets.png)

### Task 4: Investigate TCP Handshake: Investigate the TCP three-way handshake used to find open ports, focusing on ports like 445, 135, and 53.

![Figure 7 - SYN Requests and Unanswered Ports](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure07_SYN_Requests_and_Unanswered_Ports.png)

1. Noticed that the attacker sent multiple `SYN requests` to the victim machine via various `TCP ports`. Many of the requests went unanswered by the victim, indicating closed ports.

2. When the attacker machine did receive a `SYN, ACK` response from the machine, they sent back an `RST` request to terminate the communication, indicating a network mapping attack.

3. Observed such an instance when examining the results of a filter between the attacker and the victim via `Port 445`: `ip.addr == 172.31.55.149 && tcp.port == 445`.

![Figure 8 - Port 445 RST Communication](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure08_Port_445_RST_Communication.png)

### Task 5: Identify the second Attacker's IP Address from .pcap file 2: Analyze network traffic to identify the address (172.31.110.51, 172.31.43.23, 172.31.100.112, or 93.184.221.240).

![Figure 9 - PCAP File 2 Packet Examination](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure09_PCAP_File_2_Packet_Examination.png)

1. Observed that the `.pcap` file `2` has `39,827` captured packets. Investigated the following `IP` addresses using the filter `ip.addr==`:

2. `172.31.110.51` (This `IP address` appears in `39,772` packets and seems to have various communications).

3. `172.31.43.23` (This `IP address` is present in `12,119` packets but does not exhibit suspicious behavior).

4. `93.184.221.240` (This `IP address` is present in only `18` packets and does not appear suspicious).

5. `172.31.100.112` (This `IP address` shows suspicious behavior with many `TCP` connections and `RST` responses, indicating this address is the attacker’s machine).

![Figure 10 - Attacker IP 2 Filtered Traffic](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure10_Attacker_IP_2_Filtered_Traffic.png)

### Task 6: Determine Attack Time: Determine the exact time of the attack.

![Figure 11 - Attacker IP 2 Time Filter](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure11_Attacker_IP_2_Time_Filter.png)

1. Using the attacker's `IP address` filter `ip.addr== 172.31.100.112`. Changed the time display format to `Time of Day` by navigating to `View > Time Display Format > Time of Day`.

2. Observed that the time the attack began is `10:46:01`.

![Figure 12 - Attack 2 Start Time](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure12_Attack_2_Start_Time.png)

### Task 7: Investigate TCP Handshake: Investigate the attacker's TCP three-way handshake requests.

![Figure 13 - Attacker Port 21 SYN Requests](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure13_Attacker_Port_21_SYN_Requests.png)

1. Filtered the packets according to the attacker's `IP address` and the `port number` by running the command: `ip.addr==172.31.100.112 && tcp.port==21`.

2. Observed the `14` displayed packets. In all packets, the attacker's machine initiated the communication, and the victim machine did not respond.

3. Determined that the port is closed as the attacked machine does not reply to the attacker's `SYN` request.

![Figure 14 - Port 21 Closed No Response](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure14_Port_21_Closed_No_Response.png)

### Task 8: Locate Attacked Machine's IP Address: Locate the attacked machine's IP address.

![Figure 15 - Attacker Repeated Connections](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure15_Attacker_Repeated_Connections.png)

1. Noticed that the attacker repeatedly tried to connect to the `IP address 172.31.110.51`, indicating that this is the victim's `IP address`.

![Figure 16 - Victim IP Identified](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure16_Victim_IP_Identified.png)

### Task 9: Determine Open Ports: Determine which ports are open on the attacked machine: 110, 445, 21, and 3306.

![Figure 17 - Port Filtering Wireshark](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure17_Port_Filtering_Wireshark.png)

1. Filtered each port by running the command `tcp.port==`:

2. `tcp.port=110` (`14` packets sent to `port 110`, no response. `Port 110` is closed).

3. `tcp.port=21` (Similar results as `port 110`. `Port 21` is closed).

4. `tcp.port=3306` (Similar results as `ports 110` and `21`. `Port 3306` is closed).

5. `tcp.port=445` (`Port 445` responds to the attacker's `SYN` request with `SYN, ACK`. `Port 445` is open).

![Figure 18 - Open Port 445 SYN ACK](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure18_Open_Port_445_SYN_ACK.png)

### Task 10: Identify Unvisited Websites: Determine which websites were NOT visited by legitimate users: [www.msn.com](https://www.msn.com), [www.yahoo.com](https://www.yahoo.com), [www.bing.com](https://www.bing.com), and [www.google.com](https://www.google.com).

![Figure 19 - Find Packet Feature](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure19_Find_Packet_Feature.png)

1. Used the `Find Packet` feature in **Wireshark** to search for each website by navigating to `Edit > Find Packet`. Changed `Display filter` to `String` and changed `Packet list` to `Packet details`.

2. Searched for `www.msn.com`, for `www.bing.com`, and for `www.google.com`. Observed that there are packets indicating that these websites were browsed to.

3. Searched for `www.yahoo.com`. Observed that no packets contained this string, indicating that this website was not browsed and is the address that was not visited by users.

![Figure 20 - Yahoo Not Visited](https://github.com/iagsalazar1-cs/Cybersecurity-Incident-Response/blob/main/08-Network-Attack-Investigation-in-Wireshark/images/Figure20_Yahoo_Not_Visited.png)

---

### Network Attack Investigation Summary

#### Incident 1 (from .pcap file 1)

* **Attacker's IP Address:** `172.31.55.149`
* **Time of Attack:** `06:05:01`
* **Attacked Machine's IP Address:** `172.31.55.198`
* **Attacked Ports (examples):** `21`, `443`, `445` (Port `445` showed a `SYN, ACK` followed by an `RST`, indicating it was open but the attacker terminated the connection for network mapping).

#### Incident 2 (from .pcap file 2)

* **Attacker's IP Address:** `172.31.100.112`
* **Time of Attack:** `10:46:01`
* **Attacked Machine's IP Address:** `172.31.110.51`
* **Open Ports on Attacked Machine:** `445` (Ports `110`, `21`, `3306` were found to be closed as the victim did not respond to `SYN` requests).
* **Unvisited Website by Legitimate Users:** `www.yahoo.com`

---

#### Conclusion of Investigation

The investigation of two `.pcap` files revealed two distinct network attack incidents. In both cases, the attackers conducted port scanning activities, identified by multiple `SYN` requests and subsequent `RST` packets after receiving `SYN, ACK` responses, indicative of network mapping.

In the first incident, `172.31.55.149` was identified as the attacker targeting `172.31.55.198` at `06:05:01`, attempting to access various ports, including `21`, `443`, and `445`. Port `445` was found to be open.

The second incident involved `172.31.100.112` as the attacker, targeting `172.31.110.51` at `10:46:01`. In this case, port `445` was also found to be open on the victim machine, while `110`, `21`, and `3306` were closed. Additionally, it was determined that `www.yahoo.com` was the only specified website not visited by legitimate users within the captured traffic. These findings provide crucial intelligence for understanding the attack vectors and hardening the network defenses.
