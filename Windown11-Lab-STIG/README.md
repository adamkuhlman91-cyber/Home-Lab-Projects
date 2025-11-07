#Windows 11 Pro STIG Hardening Lab (Parallels Virtual Machine Run on Mac Mini M4)

##Overview
This lab demonstrates how I applied **Security Technical Implementation Guides (STIGs)** to harden a **Windows 11 Pro** virtual machine running inside **Parallels Desktop on macOS**.  
The goal was to gain hands-on experience with **system hardening**, **DoD security baselines**, and understanding how to apply **CIS and DISA guidelines** to real systems.

---

##Objectives
- Learn what STIGs are and why they matter for cybersecurity compliance  
- Practice applying several STIG recommendations to a Windows 11 system  
- Document the impact of each configuration  
- Develop familiarity with the **DoD DISA STIG viewer** and **Group Policy Editor**  

---

##Lab Environment
| Component | Configuration |
|------------|---------------|
| Host System | Mac Mini (M4 Processor, macOS Tahoe) |
| Virtualization | Parallels Desktop for Mac |
| Guest OS | Windows 11 Pro |
| Tools Used | STIG Viewer, Group Policy Editor (gpedit.msc), Windows Security, PowerShell |
| Number of STIGs Applied | 4 |

---

##STIGs Implemented
1. **Disable Autoplay for All Drives**  
   - Path: `Computer Configuration → Administrative Templates → Windows Components → AutoPlay Policies`  
   - Purpose: Prevents malware or malicious software from launching automatically when media is inserted.  

2. **Enforce Password Complexity Requirements**  
   - Path: `Computer Configuration → Windows Settings → Security Settings → Account Policies → Password Policy`  
   - Purpose: Ensures strong password enforcement aligning with DoD baseline.  

3. **Disable Guest Account**  
   - Path: `Local Users and Groups → Users`  
   - Purpose: Eliminates unauthorized guest access, a common security gap.  

4. **Require Secure Channel Communication (LDAP Signing)**  
   - Path: `Computer Configuration → Windows Settings → Security Settings → Local Policies → Security Options`  
   - Purpose: Protects authentication traffic from man-in-the-middle attacks.  

---

##Setup Summary
1. Installed Windows 11 Pro via Parallels on macOS.  
2. Downloaded **DISA STIG Viewer** and **Windows 11 Pro STIG XML** from [public.cyber.mil](https://public.cyber.mil/stigs/).  
3. Reviewed the STIG checklist and selected several baseline controls to apply manually.  
4. Applied the chosen settings via **Group Policy Editor** and **Local Security Policy**.  
5. Rebooted the VM and validated the configurations using PowerShell queries and Windows Security Center.  

---

##Verification
- Confirmed policy enforcement using `gpresult /h report.html`  
- Checked registry paths for manual verification  
- Verified that autoplay and guest accounts were fully disabled  
- Confirmed Windows Defender and firewall remained active after changes  

---

##Key Learnings
- STIGs are detailed but practical — many align with CIS Benchmarks  
- Applying even a few configurations significantly hardens a system  
- The **Group Policy Editor** is essential for local hardening and compliance  
- Understanding **why** each control exists builds better security intuition  

---

##Future Improvements
- Automate STIG application using **PowerShell DSC** or **Ansible for Windows**  
- Compare DISA STIGs vs. CIS Benchmarks  
- Apply additional STIGs related to **logging**, **BitLocker**, and **RDP restrictions**  
- Export a compliance report to include in this repo  

---

##Tools & References
| Category | Tool / Resource |
|-----------|-----------------|
| Virtualization | Parallels Desktop |
| OS | Windows 11 Pro |
| Compliance | DISA STIG Viewer |
| Hardening | Group Policy Editor, PowerShell |
| Reference | [https://public.cyber.mil/stigs/](https://public.cyber.mil/stigs/) |

---

## ✍️ Author
**Adam Kuhlman** – CompTIA Security+ Certified | Cybersecurity Home Lab Builder  
[LinkedIn Profile](https://linkedin.com/in/adamkuhlman91)
