# 💻 Lab Setup — Windows 10 Target Machine (SOC Lab)

## 📌 Overview
This section explains the complete setup of a Windows 10 target machine used in a SOC (Security Operations Center) lab environment. The purpose of this machine is to simulate a real-world endpoint that generates logs, which are then monitored and analyzed using Splunk SIEM.

This setup includes:
- Windows 10 installation
- Hostname configuration
- Splunk Universal Forwarder installation
- Sysmon deployment for advanced logging
- Log forwarding configuration using inputs.conf

---

## 🌐 Step 0 — Download Windows 10
Download the Windows 10 ISO from the official Microsoft website and attach it to VirtualBox.

👉 This ISO will be used to install the operating system inside the virtual machine.

📸 Figure 1 — Windows 10 Download Page  
![Windows Download](images/win10_figure1_download.png)

---

## 🧩 Step 1 — Install Windows 10 (VirtualBox)
- Create a new virtual machine in VirtualBox  
- Attach the Windows 10 ISO  
- Start the installation process  

### 👤 User Setup
- Username: `rocky`  
- Password: (your password)  

👉 This step creates the endpoint system that will act as a **target machine** in the SOC lab.

---

## 🖥️ Step 2 — Rename Computer
- Navigate to:
  - Settings → System → About → Rename this PC  

Set the hostname as:
```
target-PC
```

👉 Why important?
- Helps identify the machine in Splunk logs  
- Makes log analysis easier in multi-machine environments  

📸 Figure 2 — Hostname (target-PC)  
![Hostname](images/win10_figure2_hostname.png)

---

## 📦 Step 3 — Download Required Tools

### 🔹 Splunk Universal Forwarder
Used to send logs from the endpoint (Windows machine) to the Splunk Server.

📸 Figure 3 — Splunk Download Page  
![Splunk Download](images/win10_figure3_splunk_download.png)

📸 Figure 4 — Splunk Universal Forwarder Download  
![Forwarder Download](images/win10_figure4_forwarder_download.png)

👉 Role:
- Acts as a lightweight agent
- Sends logs to Splunk Indexer (SIEM)

---

### 🔹 Sysmon (System Monitor)
Sysmon is a Windows system service that logs detailed system activity.

📸 Figure 5 — Sysmon Download Page  
![Sysmon Download](images/win10_figure5_sysmon_download.png)

👉 Why Sysmon?
- Tracks process creation  
- Logs network connections  
- Detects suspicious behavior  
- Essential for threat detection  

---

### 🔹 Sysmon Configuration File
👉 [Click here for sysmonconfig.xml](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml)

📸 Figure 6 — Sysmon Config (GitHub)  
![Sysmon Config](images/win10_figure6_sysmon_config.png)

👉 This config:
- Filters unnecessary logs  
- Focuses on security-relevant events  
- Used in real SOC environments  

---

### 📁 Downloaded Files
📸 Figure 7 — Downloaded Files Folder  
![Downloaded Files](images/win10_figure7_downloaded_files.png)

👉 This ensures all required tools are ready before installation.

---

## ⚙️ Step 4 — Install Splunk Universal Forwarder
- Run the `.msi` installer  

During setup, configure:
```
Receiving Indexer: <Splunk_Server_IP>:9997
```

Example:
```
192.168.10.10:9997
```

👉 Explanation:
- `9997` = Default receiving port on Splunk server  
- This connects the endpoint to the SIEM  

📸 Figure 8 — Forwarder Installation  
![Forwarder Install](images/win10_figure8_forwarder_install.png)

---

## 🛡️ Step 5 — Install Sysmon

### Open PowerShell as Administrator
```powershell
cd <Sysmon_Directory_Path>
```

### Install Sysmon with config:
```powershell
.\Sysmon64.exe -i ..\sysmonconfig.xml
```

👉 Explanation:
- `-i` = Install Sysmon  
- `sysmonconfig.xml` = defines what to monitor  

👉 What Sysmon logs:
- Process execution (Event ID 1)  
- Network connections (Event ID 3)  
- File creation  
- Registry changes  

📸 Figure 9 — Sysmon Installation  
![Sysmon Install](images/win10_figure9_sysmon_install.png)

---

## 📄 Step 6 — Configure inputs.conf

### Create File
- Open Notepad as Administrator  
- Add Splunk input configuration  
- Save as:

```
inputs.conf
```

### Save Path
```
C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
```

👉 Explanation:
- This file tells Splunk what logs to monitor  
- Example: Sysmon logs, Windows logs  

📸 Figure 10 — inputs.conf Editing  
![Inputs Edit](images/win10_figure10_inputs_edit.png)

📸 Figure 11 — inputs.conf Path  
![Inputs Path](images/win10_figure11_inputs_path.png)

---

## ⚙️ Step 7 — Configure Splunk Forwarder Service
- Open **Services (Run as Administrator)**  
- Find: `SplunkForwarder`  
- Open properties → Log On tab  
- Select:

```
Local System Account
```

👉 Why?
- Allows access to system-level logs  
- Required for collecting security logs  

📸 Figure 12 — Service Configuration  
![Service Config](images/win10_figure12_service_config.png)

---

## 🔁 Step 8 — Restart Splunk Forwarder
```powershell
Restart-Service SplunkForwarder
```

👉 Restart is required to apply new configurations.

📸 Figure 13 — Service Running  
![Service Running](images/win10_figure13_service_running.png)

---

## 🔍 Step 9 — Verify Sysmon
```powershell
Get-Service sysmon64
```

Expected Output:
```
Status: Running
```

👉 This confirms:
- Sysmon is active  
- Logging is enabled  

📸 Figure 14 — Sysmon Status  
![Sysmon Status](images/win10_figure14_sysmon_status.png)

---

## ✅ Final Verification
- Sysmon installed and running ✅  
- Splunk Forwarder configured ✅  
- Logs being forwarded to Splunk Server ✅  

---

## 🎯 Conclusion
The Windows 10 Target Machine is successfully configured with Sysmon and Splunk Universal Forwarder. This setup enables real-time endpoint monitoring, log collection, and threat detection using the Splunk SIEM platform.

This forms a critical component of the SOC lab, simulating real-world endpoint activity for cybersecurity analysis.

---
