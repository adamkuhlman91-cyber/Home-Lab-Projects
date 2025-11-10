#Windows Server & Client Domain Controller Lab (Hyper-V)

##Overview
This lab demonstrates how I built a small enterprise-style network using **Hyper-V**, featuring a **Windows Server 2022 Domain Controller** and a **Windows 11 Pro Client** joined to the domain.  
The purpose was to learn **Active Directory (AD)**, **DNS**, **DHCP**, and domain management fundamentals — key concepts for cybersecurity and systems administration.

---

##Build Steps Completed

### Step 1 – Enable Hyper-V
- Enabled Hyper-V Platform and Management Tools via *Windows Features*.
- Verified virtualization enabled in BIOS.

### Step 2 – Download ISOs
- **Windows Server 2022 Evaluation** (Microsoft Eval Center).  
- **Windows 11 Pro** ISO (for client VM).

### Step 3 – Create Virtual Switch
- Created **Internal Switch** named `LabSwitch` in Virtual Switch Manager.  
- Verified new adapter `vEthernet (LabSwitch)` appeared in Network Connections.

### Step 4 – Create Server VM (DC01)
- Generation 2 VM, 4 GB RAM, 60 GB VHDX.  
- Installed Windows Server 2022.  
- Renamed host to `DC01`.  
- Set static IP `192.168.10.10`, DNS `127.0.0.1`.  

### Step 5 – Configure and Promote to Domain Controller
- Installed roles: **Active Directory Domain Services (AD DS)**, **DNS**, **DHCP**.  
- Promoted to domain: `homelab.local`.  
- Verified services: `adws`, `kdc`, `netlogon`, and `dns` all running.

### Step 6 – Configure DNS + DHCP
- Confirmed `homelab.local` forward lookup zone.  
- Created DHCP scope `192.168.10.100 – 192.168.10.200`.  
- Authorized DHCP server and prepared for leases to populate once client joins.

### Step 7 – Start Windows 11 Client Setup
- Created VM `CLIENT01` (4 GB RAM, 64 GB VHDX) on `LabSwitch`.  
- Resolved TPM 2.0 requirement by enabling **Virtual TPM** in Hyper-V.  
- Bypassed Microsoft-account setup using `oobe\bypassnro`.  
- Installation in progress — next phase: join domain.

---

##Suggested Screenshots
Add these to a `/screenshots` subfolder:
1. Hyper-V Manager showing both VMs (`DC01` and `CLIENT01`).
2. Server Manager roles summary (AD DS, DNS, DHCP installed).
3. DNS Manager with `homelab.local` zone.
4. DHCP scope view.
5. PowerShell IP configuration of both machines.

---

##Next Steps
1. Complete Windows 11 installation.  
2. Join `CLIENT01` to `homelab.local`.  
3. Create **Organizational Units (OUs)**, users, and Group Policies (GPOs).  
4. Apply basic **STIG or CIS hardening** to `DC01`.  
5. Configure event forwarding to **Splunk** or future SIEM lab.

---

##Tools Used
| Category | Tools |
|-----------|--------|
| Virtualization | Hyper-V |
| Server OS | Windows Server 2022 |
| Client OS | Windows 11 Pro |
| Network Services | AD DS, DNS, DHCP |
| Management | PowerShell 5.1+, Server Manager |

---

##Key Learnings
- Built foundational understanding of **Active Directory** and domain-based networking.  
- Learned how DNS and DHCP integrate with AD.  
- Practiced setting static IPs, managing roles, and verifying service health.  
- Gained hands-on experience configuring virtual networks with Hyper-V.  

---

##Author
**Adam Kuhlman** – CompTIA Security+ Certified | Cybersecurity Home Lab Builder  
[LinkedIn Profile](https://linkedin.com/in/adamkuhlman91)
