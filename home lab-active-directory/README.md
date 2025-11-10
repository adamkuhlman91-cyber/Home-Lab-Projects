#Home Lab: Windows Active Directory Environment

## Overview
This lab simulates a small enterprise environment to practice domain administration, security baselining, and endpoint management.  
Built on **Hyper-V** with an internal network, Windows Server 2022 (Domain Controller), and Windows 11 Pro (client).

---

##Environment Topology

---

##Build Steps Completed
### Step 1 – Enable Hyper-V
- Enabled Hyper-V Platform and Management Tools via *Windows Features*.
- Verified virtualization enabled in BIOS.

###Step 2 – Download ISOs
- **Windows Server 2022 Evaluation** (Microsoft Eval Center).  
- **Windows 11 Pro** ISO (for later client VM).

### Step 3 – Create Virtual Switch
- Created **Internal Switch** named `LabSwitch` in Virtual Switch Manager.  
- Verified new adapter `vEthernet (LabSwitch)` in Network Connections.

### Step 4 – Create Server VM (DC01)
- Generation 2 VM, 4 GB RAM, 60 GB VHDX.  
- Booted from Server 2022 ISO and installed successfully.

### Step 5 – Configure and Promote to Domain Controller
- Renamed host to `DC01`.  
- Set static IP 192.168.10.10, DNS 127.0.0.1.  
- Installed roles: AD DS, DNS, DHCP.  
- Promoted to domain `homelab.local`.  
- Verified services **adws**, **kdc**, **netlogon**, **dns** running.

### Step 6 – Configure DNS + DHCP
- Confirmed `homelab.local` forward lookup zone.  
- Created DHCP scope `192.168.10.100 – 192.168.10.200`.  
- Authorized DHCP server, verified active leases will appear once client joins.

### Step 7 – Start Windows 11 Client Setup
- Created `CLIENT01` VM (4 GB RAM, 64 GB VHDX) on `LabSwitch`.  
- Resolved TPM 2.0 requirement by enabling virtual TPM in Hyper-V.  
- Bypassed Microsoft-account setup using `oobe\bypassnro`.  
- Installation in progress — will join domain in next session.

---

##Screenshots (suggested)
Add these under a `/screenshots` folder:
1. Hyper-V Manager showing both VMs.  
2. Server Manager roles summary (AD DS, DNS, DHCP installed).  
3. DNS Manager with `homelab.local` zone.  
4. DHCP scope view.

---

##Next Steps
1. Finish Windows 11 installation.  
2. Join `CLIENT01` to `homelab.local`.  
3. Create Organizational Units, users, and GPOs.  
4. Document hardening/STIG process.

---

##Tools Used
- **Hyper-V** on Windows 11 Pro host  
- **Windows Server 2022 Evaluation**  
- **Windows 11 Pro ISO**  
- **PowerShell 5.1+**

---

## ✍️ Author
**Adam — Cybersecurity Lab Project**  
