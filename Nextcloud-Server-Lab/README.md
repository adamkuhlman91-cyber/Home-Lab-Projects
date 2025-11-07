#Nextcloud Server on Raspberry Pi 5

##Overview
This project demonstrates how I deployed a self-hosted **Nextcloud server** on a **Raspberry Pi 5** to create a secure, private cloud storage solution.  
The goal was to gain hands-on experience with Linux administration, networking, data backup, and self-hosting concepts.

---

##Objectives
- Set up a Nextcloud instance for personal file synchronization and storage  
- Learn how to configure storage, users, and HTTPS encryption  
- Integrate backups and logging to practice cybersecurity hygiene  
- Document the process for future automation with Docker  

---

##System Architecture
**Hardware:**
- Raspberry Pi 5 (8 GB RAM)  
- 4 TB external HD (7200 RPM NAS) connected via USB 3.0  
- Gigabit Ethernet connection  
- 65 W USB-C power supply  

**Software Stack:**
- Raspberry Pi OS Lite (64-bit)  
- Nextcloud Hub Server  
- MariaDB database  
- Nginx web server + PHP 8  
- Certbot for SSL/TLS certificates  

---

##Setup Steps Summary
1. **Flashed Raspberry Pi OS Lite** using Raspberry Pi Imager to micro SD card  
2. **Configured SSH and static IP** for headless access  
3. **Installed LAMP stack** (Nginx + MariaDB + PHP 8)  
4. **Downloaded Nextcloud package** and configured permissions  
5. **Created Nextcloud database and user** in MariaDB  
6. **Secured site with SSL certificates** using Certbot and NGINX reverse proxy  
7. **Set up daily backups to external SSD**  
8. **Tested file sync from MacBook and iPhone**  

---

##Security Practices Implemented
- HTTPS encryption via Let’s Encrypt  
- Fail2Ban installed for brute-force protection  
- Automatic updates for OS and packages  
- Database restricted to localhost only  
- Firewall rules configured via `ufw`  

---

##Key Learnings
- Hands-on experience with Linux server management  
- Understanding of port forwarding and reverse proxies  
- Real-world practice with SSL certificates and user permissions  
- Awareness of how cloud security principles apply to self-hosting  

---

##Future Improvements
- Migrate Nextcloud to Docker containers  
- Integrate Splunk forwarder to monitor Nextcloud logs  
- Automate offsite backups with rclone to Google Drive  

---

## 🧩 Tools Used
| Category | Tools |
|-----------|--------|
| OS | Raspberry Pi OS Lite |
| Web Server | Nginx |
| Database | MariaDB |
| Application | Nextcloud |
| Security | UFW, Fail2Ban, Certbot |
| Monitoring | Splunk (Planned) |

---

## ✍️ Author
**Adam Kuhlman** – CompTIA Security+ Certified | Cybersecurity Home Lab Builder  
[LinkedIn Profile](https://linkedin.com/in/adamkuhlman91)  
