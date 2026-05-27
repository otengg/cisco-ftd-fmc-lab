# 08 — Troubleshooting

This document provides a clear, structured troubleshooting guide for Cisco FTD + FMC deployments.  
These steps reflect common real-world issues seen in enterprise Firepower environments.

Use this guide whenever:

- FMC cannot register the FTD  
- Traffic does not pass  
- Logs are missing  
- IPS/URL/File inspection is not triggering events  
- NAT or ACP rules are not matching  

---

# 1. Troubleshooting FMC Registration Issues

## ❌ FMC cannot register FTD

### Possible causes:

**1. FTD cannot reach FMC**
Check connectivity:
```bash
ping <FMC-IP>
2. DNS not working

bash
Copy code
nslookup <FMC-IP>  # If FMC uses hostname
3. Wrong registration key
The key on FMC must match the one entered on FTD:

bash
Copy code
show managers
4. Time/NTP mismatch
If clocks differ significantly, FMC/FTD may fail to trust each other.

5. Firewall between FMC and FTD blocking traffic
Ensure bidirectional reachability (usually TCP 8305/443 depending on version).

2. Troubleshooting Traffic / Connectivity Issues
❌ The test client cannot reach the internet
Check:
1. Inside → Outside ACP rule exists and is deployed
Check FMC:

mathematica
Copy code
Policies → Access Control → Access Control Policies
2. NAT rule exists and is above Auto-NAT
Check:

mathematica
Copy code
Devices → NAT
3. Default route is configured on FTD

bash
Copy code
show route
4. DNS on test VM is correct

bash
Copy code
nslookup google.com
3. Troubleshooting NAT Issues
❌ NAT is not applied
Verify NAT hits:
nginx
Copy code
Analysis → Connections
Look for:

Original IP

Translated IP

Matched NAT rule

Confirm translation matches expected:
bash
Copy code
curl ifconfig.me
If the IP does not match the FTD outside IP:

PAT rule may be missing

PAT rule may be in wrong section

ACP rule might be blocking traffic

4. Troubleshooting Logging / Event Visibility
❌ No connection events in FMC
Check ACP logging:
Ensure rules are set to:

Log at End of Connection
or

Log at Beginning and End

Check FMC health:
sql
Copy code
System → Health → Monitor
Check if FTD is sending events:
php-template
Copy code
Devices → Device Management → <FTD> → Troubleshoot
5. Troubleshooting IPS Events
❌ IPS events not showing
Confirm IPS policy is applied:
nginx
Copy code
Policies → Intrusion
Ensure ACP rule is set to:
Allow

With IPS inspection

IPS Policy applied

Generate safe IPS traffic:
bash
Copy code
curl http://www.eicar.org/download/eicar.com
Expected:

Intrusion event

Malware event

File event

6. Troubleshooting URL Filtering
❌ URL categories not showing in FMC
Check URL license:
sql
Copy code
System → Licenses
Ensure ACP rule includes:
URL filtering set to Monitor or Block

Trigger URL traffic:
bash
Copy code
curl http://example.com
7. Troubleshooting Static NAT
❌ External host cannot reach internal server
Check:
Static NAT rule exists

Rule is ABOVE PAT rules

Correct port(s) are configured

Matching ACP rule allows inbound traffic

FTD outside interface is reachable

Use packet capture:

bash
Copy code
capture capin interface outside match tcp any host <Outside-IP> eq <Port>
show capture capin
8. Troubleshooting Using Packet Captures
Inside capture:
bash
Copy code
capture capin interface inside match ip host <Client> any
Outside capture:
bash
Copy code
capture capout interface outside match ip any host 8.8.8.8
View packets:
bash
Copy code
show capture capin
show capture capout
If traffic appears inside but not outside → NAT or ACP issue.
If traffic appears outside but not inside → return path / routing issue.

9. Troubleshooting Deployment Issues
❌ Changes not taking effect
Check deployments:
mathematica
Copy code
Deploy → Deployment
Look for:

Pending changes

Deployment failures

Objects out of sync

Verify FTD connectivity:
bash
Copy code
show managers
Summary
This troubleshooting guide helps diagnose the most common issues in FTD + FMC environments:

Registration failures

NAT and routing issues

ACP rule mismatches

No logs or missing events

URL, IPS, or file inspection not working

Packet-level debugging

With these steps, you can quickly isolate and resolve problems in a Firepower deployment.