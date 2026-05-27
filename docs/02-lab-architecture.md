# 02 — Lab Architecture

This document describes the logical and physical layout of the Cisco FTD + FMC lab.  
The design is intentionally simple but aligned with how Firepower is typically deployed in production.

---

# High-Level Topology

            Internet / Upstream
                    |
                    v
        +---------------------------+
        |      FTD Outside (WAN)   |
        +-------------+-------------+
                      |
                 +----+----+
                 |   FTD   |
                 | Firewall|
                 +----+----+
                      |
        +-------------+-------------+
        |       FTD Inside (LAN)    |
        +-------------+-------------+
                      |
                  Test Client
                (User / Server VM)
                      |
        +----------------------------+
        |           FMC              |
        |    Central Management      |
        +----------------------------+

---

# Roles of Each Component

## Firepower Threat Defense (FTD)

- Acts as the core firewall  
- Separates **inside** and **outside** networks  
- Enforces Access Control Policies (ACP)  
- Performs NAT (PAT and static when needed)  
- Sends logs, events, and telemetry to FMC  

---

## Firepower Management Center (FMC)

- Provides centralized management  
- Stores all security policies and objects  
- Pushes configuration to FTD  
- Provides dashboards for:
  - Connection events  
  - Intrusion events (IPS)  
  - File/Malware analysis  
  - URL filtering  
- Handles logging and reporting  

---

## Test Client

Located on the **inside** network.

Used for generating traffic such as:

- HTTP / HTTPS  
- DNS  
- ICMP  
- File downloads  

Used to validate:

- NAT  
- ACP matches  
- IPS alerts  
- URL filtering  
- Logging visibility  

---

# Networks and Interfaces (Example)

You may adjust these to match your environment.

### Inside (LAN)
- Network: `<INSIDE-SUBNET>` (e.g., 10.0.x.0/24)
- FTD Inside interface: `<FTD-INSIDE-IP>`
- Test client: `<TEST-CLIENT-IP>`

### Outside (WAN)
- Network: `<OUTSIDE-SUBNET>` (e.g., 192.168.x.0/24)
- FTD Outside interface: `<FTD-OUTSIDE-IP>`
- Upstream gateway: `<UPSTREAM-GATEWAY>`

### Management (Optional)
- FMC + FTD Management IPs
- Often placed on a dedicated VLAN  

---

# Platform Choices

This lab works on:

- Proxmox  
- VMware Workstation / ESXi  
- EVE-NG  
- CML (Cisco Modeling Labs)  
- Any hypervisor supporting FTD/FMC images  

This documentation is platform-agnostic, focusing on network/security configuration rather than hypervisor specifics.

---

# Design Goals

- Clear separation between inside/outside zones  
- Basic static routing for simplicity  
- NAT for inside → outside communication  
- Centralized logging and policy management  
- Lightweight, realistic lab layout  

---
### 🧩 CML Topology File

The full lab topology for Cisco Modeling Labs is available here:

- [`exports/FTD-FMC-lab-cml-topology.yaml`](exports/FTD-FMC-lab-cml-topology.yaml)

This YAML file can be imported directly into CML to recreate the entire environment.

---

# Next

Continue to:

`docs/03-ftd-day0-setup.md`  
