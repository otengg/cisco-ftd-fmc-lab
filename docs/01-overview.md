# 01 — Lab Overview

This lab walks through building and managing a **Cisco Firepower Threat Defense (FTD)** firewall using **Firepower Management Center (FMC)**.

The focus is on practical, real-world tasks:

- Bringing FTD online and reachable
- Registering FTD to FMC
- Building Access Control Policies (ACP)
- Configuring NAT (PAT, dynamic, static)
- Enabling IPS, URL, and file/malware inspection
- Generating traffic and analyzing events in FMC
- Troubleshooting policy and connectivity issues

The end result is an enterprise-style firewall lab that demonstrates how Cisco Firepower is used in production environments.

---

## Objectives

This lab is designed to show that I can:

- Deploy and manage Cisco FTD and FMC
- Design and apply security policies using FMC
- Use NAT and routing to control traffic flows
- Inspect and analyze security events
- Document the environment clearly and professionally

---

## Components

The lab includes:

- **FTD firewall** – main security device enforcing policy
- **FMC** – centralized manager for FTD
- **Inside network** – test client(s) behind FTD
- **Outside network** – simulated internet / upstream network

Each component is covered in its own document in this repository.

---

## How the Documentation is Organized

The `/docs` folder is structured as a sequence:

1. **01-overview** – This document (high-level goals)
2. **02-lab-architecture** – Topology and design
3. **03-ftd-day0-setup** – Initial FTD configuration
4. **04-fmc-setup-and-registration** – FMC onboarding and device registration
5. **05-access-control-policy** – Rule and policy design
6. **06-nat-configuration** – NAT rules and validation
7. **07-testing-and-event-analysis** – Verifying policies and reviewing events
8. **08-troubleshooting** – Common issues and resolution steps

You can follow them in order or jump to the section you need.

---

## Prerequisites

To reproduce this lab you should be comfortable with:

- Basic IP networking (subnets, gateways, routes)
- Basic firewall concepts (inside/outside, ACLs)
- Virtualization platform (Proxmox, VMware, EVE-NG, CML, etc.)
- Access to Cisco FTD/FMC virtual images (where licensing permits)

---

## Next

Continue to:

👉 [`02-lab-architecture.md`](02-lab-architecture.md) for the physical and logical design of the environment.
