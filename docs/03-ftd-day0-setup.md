# 03 — FTD Day 0 Setup

This document covers the initial configuration required to bring a Cisco Firepower Threat Defense (FTD) appliance online and prepare it for FMC registration.

The goal of this phase is to:

- Assign a management IP address  
- Configure DNS and time services  
- Verify network connectivity  
- Enable the system for FMC onboarding  

This is the “first boot” stage of the firewall.

---

# 1. Accessing the FTD Console

Once deployed in your hypervisor:

- Open the VM console  
- Follow the initial setup prompts  
- Set the **admin** password  
- Set the **firepower** CLI password  

These credentials are required for FMC registration.

---

# 2. Configure the Management Interface

Management is configured entirely via the CLI.

### Example:

```bash
configure network ipv4 manual <FTD-MGMT-IP> 255.255.255.0 <MGMT-GATEWAY>

Verify:
show network

Make sure:

The IP address is correct

The gateway is reachable

The management interface is up

3. Configure DNS and NTP

Accurate DNS and NTP settings are required for successful FTD → FMC communication.

Configure DNS:
configure network dns servers 1.1.1.1 8.8.8.8

Configure NTP:
configure network ntp servers pool.ntp.org

4. Verify Connectivity

Make sure the FTD can reach FMC and the internet.

Test FMC reachability:
ping <FMC-IP>

Test DNS/internet:
ping google.com


If DNS fails, correct DNS configuration before continuing.

5. Prepare for FMC Registration

Add FMC as the manager:

configure manager add <FMC-IP> <REGISTRATION-KEY>


Verify the manager status:

show managers

Expected output:

FMC IP is displayed

Status is Pending until FMC completes registration

6. Optional: System Health Validation

Enter diagnostic mode:

system support diagnostic-cli


Inside diagnostic mode:

show interface ip brief
show route


This helps confirm routing and interface status.

Summary

At the end of Day 0:

Management IP is configured

DNS and NTP are set correctly

Connectivity to FMC is verified

Registration key is added

FTD is ready for FMC onboarding

Continue to:

docs/04-fmc-setup-and-registration.md
