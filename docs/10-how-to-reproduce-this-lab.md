# 11 — How to Reproduce This Lab

This document explains how to rebuild the entire Cisco FTD + FMC Enterprise Security Lab from scratch.  
It is designed to be simple, clear, and fully reproducible.

---

# 🧱 1. Requirements

## Hardware / Virtualization  
You may use any virtualization platform:

- Proxmox  
- VMware Workstation / ESXi  
- EVE-NG  
- Cisco Modeling Labs (CML)  
- VirtualBox (not officially supported for FTD)

## Files Needed  
- Cisco FTDv image  
- Cisco FMCv image  
- Ubuntu ISO (for test client)

## Minimum Resources

| Component  | vCPU | RAM      | Storage |
|------------|------|----------|---------|
| FMC        | 4    | 8–16 GB  | 100 GB  |
| FTD        | 2–4  | 8 GB     | 20–40 GB |
| Client VM  | 1–2  | 2 GB     | 10 GB    |

---

# 🌐 2. Import the Topology

Use the provided topology diagram:

diagrams/ftd-fmc-topology.png

Recommended layout:

- Two ISP paths  
- Two edge routers  
- Two FTD firewalls  
- Dedicated management switch  
- FMC server  
- Test client on inside LAN  

Use the IP layout from:

docs/02-lab-architecture.md

---

# 🔧 3. Deploy and Configure FTD Firewalls

Use the provided Day 0 configuration files:

configs/ftd-day0-config-example.txt
configs/ftd-hq-01-bootstrap.json
configs/ftd-hq-02-bootstrap.json


## Steps:

1. Deploy the FTD image  
2. Apply the bootstrap JSON on first boot (if supported)  
3. Otherwise, manually configure the management IP  
4. Verify DNS and NTP  
5. Confirm FTD can ping FMC  
6. Add FMC as manager:

configure manager add <FMC-IP> <REG-KEY>

---

# 🧠 4. Deploy and Configure FMC

Follow:

docs/04-fmc-setup-and-registration.md


## Steps:

- Deploy FMC image  
- Complete setup wizard  
- Configure DNS + NTP  
- Apply updates  
- Create a registration key  
- Add the FTD device  
- Confirm device health is green  

---

# 🧱 5. Build Security Policies

Follow:

docs/05-access-control-policy.md


Create and deploy:

- Access Control Policy (ACP)  
- IPS policy  
- URL filtering  
- Logging configuration  

---

# 🛰 6. Configure NAT Rules

Follow:

docs/06-nat-configuration.md


Set up:

- Dynamic PAT  
- Static NAT (optional)  
- Identity NAT (optional)  
- Validate NAT in FMC  

---

# 🧪 7. Test the Environment

Follow:

docs/07-testing-and-event-analysis.md


From the test VM:

curl ifconfig.me
curl https://google.com

ping 8.8.8.8
nslookup cisco.com



Check logs in FMC:

- Connections  
- Intrusions  
- Malware  
- URL filtering  
- NAT  

---

# 🧯 8. Troubleshoot if Needed

Follow:

docs/08-troubleshooting.md


Confirm:

- FMC registration  
- NAT rule order  
- ACP rule hits  
- IPS inspection  
- Packet captures inside/outside  

---

# 🎉 Complete

You now have a fully reproduced Cisco FTD + FMC Enterprise Security Lab with:

- Centralized FMC management  
- ACP + IPS + URL + Malware inspection  
- NAT + routing  
- Full event visibility  
- Real-world test traffic  

This ensures a repeatable, enterprise-style deployment.
