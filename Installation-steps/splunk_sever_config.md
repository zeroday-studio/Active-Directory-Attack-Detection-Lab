# 🖥️ Lab Setup — Splunk Server Installation & Configuration (Ubuntu Server)

## 📌 Overview
This section covers the complete setup of Splunk Enterprise on Ubuntu Server, including downloading Ubuntu and Splunk, configuring static IP, setting up shared folders, installing Splunk, and preparing it for log collection.

---

## 🌐 Step 0 — Download Ubuntu Server (Host Machine)
- Go to Ubuntu official website  
- Download Ubuntu Server ISO  

📸 Figure 1 — Ubuntu Server Download Page  
![Ubuntu Download](images/figure1_ubuntu_download.png)

---

## 🌐 Step 0.1 — Download Splunk (Host Machine)
- Go to Splunk official website  
- Download Splunk Enterprise Free Trial (.deb package)  

📸 Figure 2 — Splunk Download Page  
![Splunk Download](images/figure2_splunk_download.png)

---

## 📂 Step 0.2 — Create Project Folder
- Create folder: `Active_Directory_Project`  
- Copy Splunk `.deb` file into this folder  

📸 Figure 3 — Project Folder  
![Project Folder](images/figure3_project_folder.png)

---

## 🧩 Step 1 — Install Ubuntu Server (VirtualBox)
- Create VM in VirtualBox  
- Attach ISO and install Ubuntu  
- Create user: `rocky`  
- Complete installation and reboot  

---

## 🌐 Step 1.1 — Configure Static IP Address

### Edit Netplan Configuration
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

📸 Figure 4 — Ubuntu Static IP Configuration  
![Static IP](images/figure4_static_ip.png)

### Apply Configuration
```bash
sudo netplan apply
```

### Verify IP Address
```bash
ip a
```

---

## ⚙️ Step 2 — Install Guest Additions
```bash
sudo apt-get update
sudo apt-get install virtualbox-guest-additions-iso
```

---

## 📂 Step 3 — Configure Shared Folder
- VirtualBox → Settings → Shared Folders  
- Add folder: `Active_Directory_Project`

📸 Figure 5 — Shared Folder Setup  
![Shared Folder](images/figure5_shared_folder.png)

---

## 👤 Step 4 — Add User to vboxsf
```bash
sudo adduser rocky vboxsf
sudo reboot
```

---

## 📁 Step 5 — Mount Shared Folder
```bash
sudo mount -t vboxsf -o uid=1000,gid=1000 Active_Directory_Project share
cd share
ls
```

📸 Figure 6 — Mount Output  
![Mount Output](images/figure6_mount.png)

---

## 📦 Step 6 — Install Splunk
```bash
sudo dpkg -i splunk*.deb
```

📸 Figure 7 — Splunk Installation  
![Splunk Install](images/figure7_install.png)

---

## 🚀 Step 7 — Start Splunk
```bash
sudo -u splunk bash
cd /opt/splunk/bin
./splunk start
```

📸 Figure 8 — Splunk Start  
![Splunk Start](images/figure8_start.png)

---

## 🔁 Step 8 — Enable Boot Start
```bash
./splunk enable boot-start -user splunk
```

---

## 🌐 Step 9 — Access Splunk Web
- Open browser:
```
http://192.168.10.10:8000
```

📸 Figure 9 — Splunk Login  
![Splunk Login](images/figure9_login.png)

📸 Figure 10 — Splunk Dashboard  
![Dashboard](images/figure10_dashboard.png)

---

## 📊 Step 10 — Create Index
- Go to: Settings → Indexes → New  

📸 Figure 11 — Index Creation  
![Index](images/figure11_index.png)

---

## 🔌 Step 11 — Configure Receiving Port
- Go to: Settings → Forwarding & Receiving  
- Add port: `9997`

📸 Figure 12 — Receiving Port  
![Port](images/figure12_port.png)

---

## 🔍 Step 12 — Log Search in Splunk
- Go to: Apps → Search & Reporting  
- Use search query:

```spl
index=endpoint
```

---

## 🔗 Step 13 — Forwarders Connected to Splunk
- Domain Controller (AD Server)  
- Windows 10 Target Machine  

📸 Figure 13 — Connected Forwarders  
![Forwarders](images/figure13_forwarders.png)

> ⚠️ **Note:** This will be visible only after both systems (AD Server and Target PC) are successfully configured with Splunk Forwarders and connected to the Splunk Server. Please verify after adding both systems.

---

## ✅ Conclusion
This confirms that both machines are successfully forwarding logs to the Splunk server, enabling centralized monitoring and analysis in the SOC environment.

---
