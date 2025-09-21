# Suspicious logon times - After Hour Admin login (Windows 8 + Splunk)

This lab simulates a Suspicious logon by Admin accounts after office hours, detects it by using Splunk. It follows MITRE ATT&CK techniques:
- T1078.004: Valid Accounts.
---

## 🛠️ Lab Setup

### Machines
- **Attacker**: Kali Linux (Hydra, smbclient)
- **Victim**: Windows 8.1 (Build 9600) with:
  - Sysmon
  - Splunk Universal Forwarder
- **SIEM**: Splunk Enterprise (host machine)


## 🧪 Attack Steps
- What I did: Simulated privileged logins outside normal business hours.

- Detection logic: Enrich login events with a business-hours lookup (CSV of normal hours). Alert when successful privileged logons occur outside those windows and from new IPs:

   ```spl
    index=main EventCode=4624 Account_Name="Administrator"
    | lookup business_hours user AS Account_Name OUTPUT start_hour, end_hour
    | eval hour=strftime(_time,"%H")
    | where hour < start_hour OR hour > end_hour
   ```

- Why it matters: Flags potentially compromised credentials or misuse of admin accounts (maps to T1078 / anomalous behavior).