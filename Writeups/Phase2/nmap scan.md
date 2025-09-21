# Nmap — port/open service scanning (T1046) Detection (Windows 8 + Splunk)

This lab simulates a nmap scanning (Active Reconnaisance) on a Windows 8.1 VM using the nmap  tool in kali linux and detects it using Splunk. It follows MITRE ATT&CK techniques:
- T1046 - Network Service Scanning

---

## 🛠️ Lab Setup

### Machines
- **Attacker**: Kali Linux (Hydra, smbclient)
- **Victim**: Windows 8.1 (Build 9600) with:
  - Sysmon
  - Splunk Universal Forwarder
- **SIEM**: Splunk Enterprise (host machine)

### Tools Used
- `nmap`: Port scanning/service enumeration
- `Sysmon`: for process and network monitoring
- `Splunk`: for log ingestion and analysis

---

## 🧪 Attack Steps

- Ran nmap -sS -sV -p- <target> from Kali to enumerate services.

- Detection: Used Sysmon network connection events (EventCode=3) in Splunk and counted distinct destination ports per source IP in short windows:

- Investigate in Splunk

**Event code - 3 : System Network Connections**

Set the time frame to **last 15 mins / last 60 mins:**
   ```spl
   index=main EventCode=3
|  bucket _time span=1m
|  stats dc(dest_port) AS unique_ports by src_ip, _time
|  where unique_ports > 20

   ```