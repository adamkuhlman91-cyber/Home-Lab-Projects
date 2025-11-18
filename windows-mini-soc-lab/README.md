# Windows Mini SOC Lab – AD + Splunk

This project is a miniature enterprise environment to practice **SOC (Security Operations Center) skills**:

- Windows Server 2022 domain controller (DC01)
- Windows 11 client (CLIENT01)
- Active Directory, DNS, DHCP
- Splunk Enterprise (on DC01)
- Splunk Universal Forwarder (on CLIENT01)
- Internal-only Hyper-V network (`LabSwitch`)

The goal is to mimic the type of endpoints and telemetry a real SOC analyst would work with:  
**Windows auth logs flowing into a SIEM (Splunk), on an isolated lab network.**

---

## 1. Lab Topology

**Host:** HP ProDesk 600 G6 (Hyper-V host)

**Virtual Switch**

- `LabSwitch` – Hyper-V **Internal** switch (no direct internet)
- Purpose: internal LAN for DC01, CLIENT01, and future lab VMs

**VMs**

| Role                | Hostname        | IP Address      | OS                    | Main Services                         |
|---------------------|-----------------|-----------------|-----------------------|---------------------------------------|
| Domain Controller   | `DC01`          | `192.168.50.10` | Windows Server 2022   | AD DS, DNS, DHCP, Splunk Enterprise  |
| Windows Client      | `CLIENT01`      | `192.168.50.100`| Windows 11            | Domain-joined workstation, Splunk UF |

**IP Scheme**

- Network: `192.168.50.0/24`
- DC01: `192.168.50.10`
- DHCP scope: `192.168.50.100–192.168.50.200`
- Client example: `192.168.50.100` (assigned via DHCP)
- DNS: DC01 (self), then used by clients

---

## 2. Build Steps (High Level)

### 2.1 Hyper-V Host Prep

1. Enable **Hyper-V** in Windows Features.
2. Create an **Internal** virtual switch:

   - Name: `LabSwitch`
   - Type: **Internal network**
   - This gives the host and all VMs a private network that **does not break host internet**.

3. Confirm `vEthernet (LabSwitch)` appears in Network Connections.

---

### 2.2 Create DC01 (Domain Controller)

1. New VM in Hyper-V:

   - Name: `DC01`
   - Gen 2, ~4 GB RAM, 60+ GB disk
   - Attach Windows Server 2022 ISO
   - Network adapter: `LabSwitch`

2. Install Windows Server 2022.
3. Set static IP on DC01:

   - IP: `192.168.50.10`
   - Mask: `255.255.255.0`
   - **Default gateway:** (leave blank – no internet in this lab)
   - DNS: `127.0.0.1` (or `192.168.50.10` – see *lessons learned*)

4. Rename computer → `DC01` and reboot.

5. Install roles:

   - **Active Directory Domain Services**
   - **DNS Server**
   - **DHCP Server**

6. Promote to domain:

   - New forest: `homelab.local`
   - After reboot, log in as domain admin.

7. Configure **DHCP**:

   - Scope: `192.168.50.100–192.168.50.200`
   - Subnet mask: `255.255.255.0`
   - Router (gateway): blank
   - DNS: `192.168.50.10`
   - Activate the scope.

8. Configure **DNS**:

   - Ensure forward lookup zone: `homelab.local`
   - Add **reverse lookup zone**: `50.168.192.in-addr.arpa` (for `192.168.50.0/24`)
   - Verify `A` and `PTR` records for `DC01`.

9. Basic tests on DC01:

   ```cmd
   ipconfig /all
   nslookup dc01.homelab.local
   nslookup 192.168.50.10


⸻

2.3 Create CLIENT01 (Windows 11)
	1.	New VM in Hyper-V:
	•	Name: CLIENT01
	•	Attach Windows 11 ISO
	•	Network adapter: LabSwitch
	2.	Install Windows 11.
	3.	Confirm NIC is on LabSwitch and using DHCP.
	4.	Verify IP and DNS:

ipconfig /all

Expect something like:
	•	IP: 192.168.50.100
	•	DHCP server: 192.168.50.10
	•	DNS server: 192.168.50.10
	•	DNS suffix: homelab.local

	5.	Join the domain:
	•	System → Rename this PC → Domain: homelab.local
	•	Use domain admin credentials.
	•	Reboot and log in as HOMELAB\<user>.
	6.	Name registration & tests:
On CLIENT01:

ipconfig /registerdns

On DC01:

nslookup client01.homelab.local
ping client01.homelab.local

Note: ICMP may be blocked by firewall even when DNS works – see lessons learned.

⸻

3. Splunk Setup

3.1 Splunk Enterprise on DC01
	1.	Download Splunk Enterprise for Windows (64-bit).
	2.	Install on DC01 with defaults.
	3.	Choose admin credentials (example):
	•	Username: admin77
	•	Strong password.
	4.	After install, open Splunk Web on DC01:
	•	URL: http://127.0.0.1:8000
	•	Log in with admin77.
	5.	Enable port 9997 for receiving from forwarders:
In an elevated PowerShell on DC01:

cd "C:\Program Files\Splunk\bin"
.\splunk.exe enable listen 9997 -auth admin77:YourPasswordHere


	6.	Confirm Splunk is listening:

netstat -an | findstr 9997

Should show: TCP 0.0.0.0:9997 LISTENING.

⸻

3.2 Universal Forwarder on CLIENT01
	1.	Download Splunk Universal Forwarder for Windows (64-bit).
	2.	Install on CLIENT01:
	•	During setup, when asked for deployment server / receiving indexer, point to 192.168.50.10 where appropriate, or skip and configure manually.
	3.	Edit outputs.conf on CLIENT01:
Path (default):

C:\Program Files\SplunkUniversalForwarder\etc\system\local\outputs.conf

Contents:

[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = 192.168.50.10:9997


	4.	Edit inputs.conf to send Windows event logs:

C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf

Contents:

[WinEventLog://Security]
disabled = 0
index = wineventlog

[WinEventLog://System]
disabled = 0
index = wineventlog

[WinEventLog://Application]
disabled = 0
index = wineventlog


	5.	Restart the Splunk Forwarder service on CLIENT01:

Restart-Service SplunkForwarder


	6.	Verify forwarder status:

cd "C:\Program Files\SplunkUniversalForwarder\bin"
.\splunk.exe list forward-server

Expected:

Active forwards:
    192.168.50.10:9997
Configured but inactive forwards:
    None



⸻

3.3 Configure Index in Splunk (DC01)
	1.	In Splunk Web on DC01, go to:
	•	Settings → Indexes → New Index
	2.	Create an index:
	•	Name: wineventlog
	3.	Search for events:
	•	App: Search & Reporting
	•	Example Splunk Search:

index=wineventlog host=CLIENT01


	4.	Generate some test events:
	•	Log off / log on CLIENT01.
	•	Intentionally fail a few logins.
	•	Open Event Viewer, etc.

You should now see Windows event logs from CLIENT01 in Splunk on DC01.

⸻

4. Mistakes & What I Learned

This lab was not plug-and-play. The value is in the problems that were solved along the way.

4.1 DNS Misconfiguration on DC01
	•	Initially DC01’s IPv4 config had:
	•	IP: 192.168.50.10
	•	DNS: 127.0.0.1
	•	Reverse lookup zone was missing.
	•	CLIENT01 was getting an IP but DNS lookups for client01.homelab.local failed.
	•	Symptoms:
	•	nslookup client01 on DC01 → Non-existent domain
	•	Splunk forwarder working at the network level but name resolution broken.

Fix / Lessons
	•	Create the reverse lookup zone 50.168.192.in-addr.arpa.
	•	Ensure A and PTR records exist for both DC01 and CLIENT01.
	•	Use:

ipconfig /registerdns
nslookup <hostname>
nslookup <ip>


	•	Watching nslookup go from Unknown to properly resolving client01.homelab.local was the turning point.

4.2 Pings Timing Out
	•	Even after DNS was fixed, ping CLIENT01 from DC01 failed.
	•	DNS resolution worked, but ICMP requests timed out.

Lesson
	•	This is normal in many environments: Windows Firewall can block ICMP.
	•	Name resolution and application traffic can still work even when ping fails.
	•	Real SOC work often involves understanding that “ping fails” ≠ “host is down.”

4.3 Splunk Events Not Appearing

Several problems hit at once:
	1.	Forwarder configured but inactive
	•	splunk.exe list forward-server on CLIENT01 initially showed:
	•	Configured but inactive forwards: 192.168.50.10:9997
	2.	Listening port not set correctly on DC01
	•	9997 wasn’t enabled at first.
	•	Fix: splunk.exe enable listen 9997 -auth admin77:...
	3.	Index mismatch
	•	Forwarder was sending to index=wineventlog.
	•	That index didn’t exist yet in Splunk Enterprise.
	•	Once the wineventlog index was created, events finally showed up.

Key Lessons
	•	Always check three things when logs aren’t appearing:
	1.	Forwarder status (list forward-server)
	2.	Listening port on indexer (netstat -an | findstr 9997)
	3.	Correct index name (inputs vs Splunk index configuration)
	•	Editing inputs.conf and outputs.conf manually is normal in the field and a good skill to have.

⸻

5. Skills Demonstrated

By finishing this lab, I practiced:
	•	Building an isolated Hyper-V lab that doesn’t break the host’s internet
	•	Configuring a Windows Server 2022 domain controller (AD DS, DNS, DHCP)
	•	Domain-joining clients and troubleshooting DNS
	•	Understanding A/PTR records, reverse lookup zones, and nslookup
	•	Installing and configuring Splunk Enterprise
	•	Installing and configuring the Splunk Universal Forwarder
	•	Debugging log ingestion (ports, indexes, config files)
	•	Writing basic Splunk searches to validate ingestion

This is the same foundation junior SOC analysts use to understand how Windows telemetry reaches the SIEM.

⸻

6. Next Steps / Roadmap

Planned enhancements:
	•	Install Splunk Universal Forwarder on DC01 itself for domain controller logs
	•	Install Sysmon on CLIENT01 and ingest Sysmon logs into Splunk
	•	Add Ubuntu or Kali as attacker / web server machines
	•	Create Splunk dashboards:
	•	Failed logon dashboard (EventCode 4625)
	•	Successful logon timeline (4624)
	•	Account lockouts, service installation, privilege changes
	•	Simulate basic attacks (brute force, lateral movement) and detect them in Splunk

⸻


---

## 2️⃣ Update your main `README.md` to link this lab

Now link this project from your root README so it actually works.

1. In GitHub, open `README.md` in the root of `Home-Lab-Projects`.
2. Click **Edit**.
3. Wherever you list your labs, add a bullet like this:

```markdown
- [Windows Mini SOC Lab – AD + Splunk](./windows-mini-soc-lab/README.md)

	4.	Commit the change.

That relative link will work as long as the folder name matches windows-mini-soc-lab.

⸻

If you want, next step after this is done, we can add a “How to talk about this lab in an interview” section, or a shorter “portfolio summary” you can paste into LinkedIn / resume.
