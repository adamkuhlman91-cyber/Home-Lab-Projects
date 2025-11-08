#Windows 11 System Baseline & Patch Management Lab

##Overview
This lab documents the initial setup and patching of a refurbished HP ProDesk 600 G6 desktop system used for home lab projects.  
The goal is to establish a **secure baseline** before deploying virtual machines, SIEM tools, or server roles.

---

##Hardware Specs
- HP ProDesk 600 G6 Microtower  
- Intel Core i5-10500  
- 512 GB SSD, 16 GB RAM  
- Windows 11 Pro  

---

##Initial Setup Steps
1. Booted into Windows 11 for the first time  
2. Configured Microsoft account and system preferences  
3. Installed all available Windows updates and firmware patches  
4. Verified device health and driver integrity through **Device Manager** and **Windows Update history**  

---

##Updates & Patches Installed
*(as of November 8, 2025)*  
| Update | Description | Status |
|---------|--------------|--------|
| KB2267602 | Security Intelligence Update for Microsoft Defender | ✅ Installed |
| KB890830 | Windows Malicious Software Removal Tool | ✅ Installed |
| KB5007651 | Windows Security Platform Update | ✅ Installed |
| Intel Display Driver | v31.0.101.2135 | ✅ Installed |
| HP Firmware | v2.23.0.0 | ✅ Installed |
| Preview Update KB5067036 | Optional Feature Update | ❌ Skipped (non-security) |

---

##Verification
- Confirmed all updates completed successfully under *Windows Update → Update History*  
- Checked firmware version in **Device Manager → Firmware**  
- Confirmed Defender up to date via `Get-MpComputerStatus` in PowerShell  

---

##Key Learnings
- Establishing a system baseline is crucial before applying STIGs or installing additional software  
- Firmware updates are just as critical as OS patches for system stability and security  
- Optional “preview” updates are often skipped in production-grade setups unless needed for testing  

---

##Next Steps
- Create a **baseline system image snapshot**  
- Begin **Active Directory / Splunk / SIEM lab deployments** on this system  
- Configure **Windows Defender Exploit Guard** and additional STIG policies  

---

##Screenshot
*(Initial patching process)*
![Windows Update Screenshot](./windows-update.png)
