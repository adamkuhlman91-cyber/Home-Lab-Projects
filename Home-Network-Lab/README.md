#Home Network Overhaul & Security Segmentation Lab

##Overview
This project documents how I completely overhauled my home network to improve wireless coverage, performance, and cybersecurity.  
The goal was to gain hands-on experience with **Wi-Fi 6/7 mesh systems**, **network segmentation**, and **security hardening practices** for a real-world environment.

---

##Objectives
- Replace older Eero routers with modern Wi-Fi 7 mesh systems for better coverage  
- Create **separate network segments** for main devices, IoT devices, and guests  
- Implement basic **zero-trust principles** by isolating IoT traffic  
- Regularly monitor for firmware updates and security vulnerabilities  
- Document setup for use as a repeatable home lab exercise  

---

##System Architecture

###Hardware
| Device | Model | Standard | Role |
|--------|--------|-----------|------|
| Indoor Mesh Nodes | TP-Link Deco BE23 (x2) | Wi-Fi 7 (BE3600 Dual Band) | Main and satellite nodes for indoor coverage |
| Outdoor Node | TP-Link Deco X20-Outdoor | Wi-Fi 6 (AX1800 Dual Band) | Extends mesh to backyard and exterior areas |

###Network Topology

---

##Network Segmentation
| Network Name | Purpose | Security Settings |
|---------------|----------|------------------|
| **Main Network** | Trusted personal devices (computers, phones, tablets) | WPA3, MAC filtering enabled |
| **Guest Network** | Visitors and temporary access | WPA2, client isolation enabled |
| **IoT Network** | Smart home devices (TVs, cameras, plugs, appliances) | Separated VLAN via Deco configuration |

---

##Security Hardening Steps
- Enabled **TP-Link Security+** for threat detection, intrusion prevention, and malicious site blocking  
- Changed default admin credentials and enabled **two-factor authentication (2FA)**  
- Configured **automatic firmware updates**  
- Disabled WPS and remote management when not needed  
- Regularly review device list and connection logs  
- Firewall rules adjusted to prevent IoT-to-main network access  

---

##Network Monitoring & Maintenance
- Monthly review of connected devices  
- Manual speed tests and latency checks using `speedtest-cli`  
- Periodic firmware update checks via TP-Link app  
- Future goal: integrate **Splunk forwarder or network monitoring dashboard** (e.g., Grafana/Prometheus)  

---

##Key Learnings
- Practical understanding of **mesh topology** and **Wi-Fi standards**  
- How **network segmentation** reduces attack surface  
- Importance of continuous patch management  
- Hands-on experience applying **real-world home security principles**  

---

##Future Improvements
- Add VLAN tagging via managed switch for more granular segmentation  
- Implement local DNS filtering (e.g., Pi-hole or AdGuard Home)  
- Deploy a Raspberry Pi syslog server to centralize router logs  

---

## 🧩 Tools & Technologies
| Category | Tools |
|-----------|--------|
| Hardware | TP-Link Deco BE23, Deco X20-Outdoor |
| Network Standards | Wi-Fi 6, Wi-Fi 7 |
| Security Suite | TP-Link Security+ |
| Monitoring | Speedtest CLI, TP-Link App |
| Planned Integrations | Pi-hole, Splunk, Grafana |

---

## ✍️ Author
**Adam Kuhlman** – CompTIA Security+ Certified | Cybersecurity Home Lab Builder  
[LinkedIn Profile](https://linkedin.com/in/adamkuhlman91)
