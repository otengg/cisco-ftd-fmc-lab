# 09 — Device Inventory & Version Information

This document provides a clear inventory of all devices used in the Cisco FTD + FMC Enterprise Security Lab.  
It includes hardware/virtual specs, interface mappings, software versions, and functional roles.  
This format reflects common enterprise documentation standards.

---

# 1. Firepower Management Center (FMC)

**Name:** FMC-HQ  
**Role:** Centralized management for all Firepower devices  
**Deployment Type:** Virtual appliance  
**CPU:** 4 vCPU  
**RAM:** 8–16 GB  
**Storage:** 100+ GB  
**Software Version:** (Fill in your FMC version, e.g., 7.2.5)  

### Interfaces:
| Interface | Function           | IP Address         | Notes                        |
|----------|--------------------|--------------------|------------------------------|
| eth0     | Management         | `<FMC-MGMT-IP>`   | Required for FTD registration |

---

# 2. Firepower Threat Defense — FTD-HQ-01

**Name:** FTD-HQ-01  
**Role:** Primary perimeter firewall  
**Deployment Type:** Virtual appliance  
**CPU:** 2–4 vCPU  
**RAM:** 8 GB  
**Software Version:** (Add your FTD version)  

### Interfaces:
| Interface | Function         | IP Address        | Notes                   |
|----------|------------------|-------------------|-------------------------|
| Mgmt0/0  | Management       | DHCP / Bootstrapped | Used for FMC registration |
| G0/0     | Outside (WAN)    | DHCP / or static  | Connects to HQ-EDGE-R1   |
| G0/1     | Inside (LAN)     | `<FTD-INSIDE-IP>` | Default gateway for LAN |
| G0/2     | Optional/DMZ     | —                 | Future expansion         |

---

# 3. Firepower Threat Defense — FTD-HQ-02

**Name:** FTD-HQ-02  
**Role:** Secondary firewall / HA-ready  
**Deployment Type:** Virtual appliance  
**CPU:** 2–4 vCPU  
**RAM:** 8 GB  
**Software Version:** (Add your FTD version)  

### Interfaces:
| Interface | Function         | IP Address        | Notes                     |
|----------|------------------|-------------------|---------------------------|
| Mgmt0/0  | Management       | DHCP / Bootstrapped | Used for FMC registration |
| G0/0     | Outside (WAN)    | DHCP / or static  | Connects to HQ-EDGE-R2     |
| G0/1     | Inside (LAN)     | `<FTD-INSIDE-IP-2>` | (If used for segmented LAN) |
| G0/2     | Optional/DMZ     | —                 |              |

---

# 4. HQ Edge Routers

These simulate upstream ISP routers or enterprise WAN edge.

### HQ-EDGE-R1
- **Function:** Upstream router / WAN path 1  
- **Interfaces:**  
  - G0/0 → ISP-SW  
  - G0/1 → FTD-HQ-01 Outside  

### HQ-EDGE-R2
- **Function:** Upstream router / WAN path 2  
- **Interfaces:**  
  - G0/0 → ISP-SW  
  - G0/1 → FTD-HQ-02 Outside  

---

# 5. Management Switch (MGMT-SW)

**Role:** Dedicated management segment  
**Purpose:** Isolates FMC + FTD management interfaces  

### Sample Mappings:
| Device | Interface | Switch Port | Notes |
|--------|-----------|-------------|-------|
| FMC-HQ | eth0      | E0          | Management VLAN |
| FTD-HQ-01 | Mgmt0/0 | Mgmt0       | To FMC |
| FTD-HQ-02 | Mgmt0/0 | Mgmt0       | To FMC |

---

# 6. HQ-Core Switch (HQ-CORE-SW1)

**Role:** Internal LAN switch  
**Purpose:** Provides inside network access to test clients  

### Sample Ports:
| Port  | Connected To    | Notes          |
|-------|-----------------|----------------|
| E0/0  | FTD-HQ-01 G0/1  | Inside LAN     |
| E0/1  | FTD-HQ-02 G0/1  | (Optional)     |
| E0/2  | Test Client     | `<TEST-CLIENT-IP>` |

---

# 7. Test Client (Ubuntu VM)

**Name:** ubuntu-0  
**Role:** Traffic generator + validation endpoint  
**CPU:** 1–2 vCPU  
**RAM:** 2 GB  
**OS:** Ubuntu 22.04 / your version  
**IP Address:** `<TEST-CLIENT-IP>`  

### Used for:
- NAT testing  
- URL filtering  
- IPS triggers  
- Connection event verification  
- Packet capture validation  

---

# 8. ISP Simulation

### ISP-SW
- **Function:** Simple Layer 2 distribution switch for ISP paths

### ISP Cloud Nodes
- **Function:** Simulated internet (2 ISPs)
- Verizon ISP  
- Cogent ISP  

These provide:
- Redundant WAN paths  
- Routing realism  
- Failover simulation possibilities  

---

# Summary

This inventory provides:

- A complete snapshot of all virtual devices  
- Interface and role mapping  
- Software and version baselines  
- A realistic enterprise-style documentation structure  

This document supports change management, troubleshooting, and future lab expansions.
