#Microsoft Azure Virtual Machine Deployment Lab

##Overview
This project documents how I created and secured a **Microsoft Azure Virtual Machine (VM)** to gain hands-on experience with **cloud infrastructure**, **network configuration**, and **virtualization security**.  
The goal was to understand how cloud resources are provisioned, managed, and protected — foundational skills for both cloud and cybersecurity professionals.

---

##Objectives
- Learn the basics of creating and managing an Azure VM  
- Configure inbound/outbound network rules for security  
- Practice remote administration using SSH and RDP  
- Explore resource monitoring and billing management in Azure  
- Document the full setup for repeatability and learning  

---

##System Architecture

###VM Configuration
| Parameter | Value |
|------------|--------|
| Cloud Platform | Microsoft Azure |
| VM Type | Standard B1s |
| OS | Windows Server 2022 |
| Region | East US |
| Storage | 30 GB Managed Disk |
| Public IP | Dynamic |
| Authentication | SSH key / RDP credentials |

---

##Security Configuration
- Configured **Network Security Group (NSG)** with inbound rule for **RDP (3389)** or **SSH (22)** only from my home IP address  
- Disabled all other inbound ports by default    
- Installed updates immediately after deployment  
- Created a strong admin password and disabled default accounts  
- Enabled **Microsoft Defender for Cloud** to monitor threats  

---

##Hands-On Steps Summary
1. Logged into Azure Portal and navigated to **Virtual Machines**  
2. Clicked **Create → Azure Virtual Machine**  
3. Selected resource group, VM name, and image  
4. Configured size (B1s), region (East US), and admin credentials  
5. Added inbound rule for SSH/RDP from home IP only  
6. Deployed and verified connectivity via terminal or RDP  
7. Installed tools for testing (PowerShell, ping, Nmap, etc.)  
8. Monitored resource usage via **Azure Monitor** and **Activity Log**  

---

##Monitoring and Maintenance
- Checked metrics using **Azure Monitor** and **Log Analytics**  
- Configured **budget alerts** to avoid unexpected costs  
- Tested connectivity from different networks to confirm firewall rules  
- Shut down VM when not in use to save credits  

---

##Key Learnings
- Understanding of **Azure resource hierarchy** (subscription, resource group, VM)  
- Practical exposure to **cloud networking and access control**  
- Reinforced security mindset — “deny all, then allow only what’s needed”  
- Confidence working within the Azure Portal interface  

---

##Future Improvements
- Automate VM deployment using **Azure CLI** or **Terraform**  
- Integrate with **Azure Sentinel (SIEM)** for log analysis  
- Build a **honeypot VM** to study brute-force attempts and log events  
- Connect to home lab network via VPN for hybrid-cloud testing  

---

##Tools & Technologies
| Category | Tools |
|-----------|--------|
| Cloud Platform | Microsoft Azure |
| OS | Windows Server 2022 / Ubuntu 22.04 |
| Security | NSG, Microsoft Defender for Cloud |
| Admin Tools | RDP, SSH, PowerShell |
| Monitoring | Azure Monitor, Log Analytics |

---

## ✍️ Author
**Adam Kuhlman** – CompTIA Security+ Certified | Cybersecurity Home Lab Builder  
[LinkedIn Profile](https://linkedin.com/in/adamkuhlman91)
