# wireshark-arp-traffic-analysis
Analyze ARP traffic between two virtual machines (Kali Linux attacker + Windows 11 victim) to understand how ARP works, identify ARP broadcasts, and observe ARP request/response patterns on the network.

**Lab Environment**
- Windows 11 (Victim / Packet Capture) Running Wireshark
- Kali Linux (Attacker / Traffic Generator) Used to flush ARP cache and generate new ARP activity
- VMware Workstation Pro Both VMs on the same NAT/Bridged network

**1. Flushed ARP Cache on Kali**
Command used: sudo ip neigh flush all
This forces the system to forget previously learned MAC → IP mappings, ensuring new ARP broadcasts appear when packets are sent.

**2. Ping From Kali → Windows**
Command used: ping -c 1 192.168.58.129
Purpose: Trigger ARP packets because Kali must resolve the MAC address of the Windows host before ICMP traffic can be sent.

<img width="2348" height="1330" alt="Screenshot 2025-11-20 154149" src="https://github.com/user-attachments/assets/0cd38ad0-fdac-474b-9ba3-652c82db8fdb" />

Captured Traffic in Wireshark (Windows)
Filter applied: arp
This displays only ARP packets, making it easier to observe: ARP requests
ARP replies
Source/destination MAC relationship
Broadcast behavior

<img width="2350" height="1282" alt="Screenshot 2025-11-20 154229" src="https://github.com/user-attachments/assets/2c9d35ed-5f48-41a1-bc87-70b0d592c9bf" />

**Findings**
ARP Request:
“Who has target_IP? Tell source_IP”
Broadcast to: ff:ff:ff:ff:ff:ff

ARP Reply:
“target_IP is at MAC address” (unicast)
Clearing ARP cache ensured fresh ARP traffic.
Wireshark clearly showed VMware MAC prefixes and normal ARP behavior.

**MITRE ATT&CK**

T1040 – Network Sniffing

T1557 – Adversary-in-the-Middle (ARP Spoofing)

**SUMMARY**

This lab demonstrates basic network analysis by generating ARP traffic, capturing it with Wireshark, and identifying ARP request/reply behavior—skills relevant to SOC and cybersecurity roles.
