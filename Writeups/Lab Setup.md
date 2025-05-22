**Building a Personal SOC Lab: Step-by-Step Setup and Logging Guide**

---

### 📁 Introduction

Setting up your own Security Operations Center (SOC) lab is a great way to learn cybersecurity detection, monitoring, and investigation using real tools. In this guide, I walk through the full setup of a basic lab consisting of a Windows 8 target VM, a Kali Linux attacker VM, and Splunk as the SIEM running on the host machine.

---

## 🚀 Lab Environment Overview

* **Target Machine**: Windows 8 VM
* **Attacker Machine**: Kali Linux VM
* **SIEM/Log Collector**: Splunk Free running on Host
* **Log Forwarding Tool**: Splunk Universal Forwarder
* **Monitoring Tool**: Sysmon

---

## 📅 Step 1: Setting Up Virtual Machines

### ✅ Install VMware Workstation (or any hypervisor):

1. Download and install VMware Workstation Player (free for personal use).
2. Create two virtual machines:

   * **Windows 8 VM**: Install Windows 8 ISO and complete the setup.
   * **Kali Linux VM**: Download and install the latest Kali ISO from the official site.

### ⚖️ Network Configuration:

* Use **NAT or Bridged** mode so that all VMs can communicate with each other and the host.

## Lab Setup Screenshot :
![Description for image 1](C:\Users\himap\Bug-Bounty\siem-internship-phase-1\Screenshots\image1.png)

---

## 🌍 Step 2: Setting Up the SIEM (Splunk on Host)

### 🔧 Install Splunk Free:

1. Download Splunk from [https://www.splunk.com](https://www.splunk.com)
2. Install using default settings.
3. Launch Splunk from the browser at `http://localhost:8000`
4. Create an admin account when prompted.

---

## 🚪 Step 3: Configuring the Windows 8 Target VM

### 🔢 Install Sysmon:

1. Download Sysmon from Sysinternals: [https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
2. Extract Sysmon and open Command Prompt as Administrator.
3. Run:

   ```bash
   Sysmon.exe -accepteula -i sysmonconfig-export.xml
   ```

   *(Make sure you have a Sysmon config file. You can use SwiftOnSecurity's config or generate your own.)*
4. Confirm Sysmon service is running:

   ```bash
   sc query sysmon
   ```

### 🛠️ Enable Event Logs:

Check that these logs are enabled via Event Viewer:

* Security
* System
* Application
* Windows PowerShell
* Sysmon (under Applications and Services Logs > Microsoft > Windows > Sysmon)

---

## 💡 Step 4: Install Splunk Universal Forwarder on Windows 8

1. Download the Universal Forwarder from Splunk: [https://www.splunk.com/en\_us/download/universal-forwarder.html](https://www.splunk.com/en_us/download/universal-forwarder.html)
2. Install with admin privileges.
3. During setup:

   * Point to your Splunk host machine's IP and port `9997`
   * Choose "Send Windows Event Logs"
4. After installation, confirm that Splunk UF service is running:

   ```bash
   services.msc
   ```
5. On Splunk (host machine), enable data input:

   * Go to **Settings > Data Inputs > Forwarded Data**
   * Add port `9997`

---

## 📊 Step 5: Confirming Log Ingestion in Splunk

1. In the Splunk Search app, run:

   ```spl
   index=* host=<windows8-hostname>
   ```
2. You should see logs coming in from Sysmon and Event Logs.

---

## 📚 Conclusion

You now have a functioning SOC lab with:

* A monitored Windows target system
* Sysmon tracking critical activity
* Logs forwarded to Splunk in real time
* Ready environment for attack simulation and detection development

Stay tuned for attack simulation steps and detection rule creation!
