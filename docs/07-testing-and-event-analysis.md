# 07 — Testing & Event Analysis

This document explains how to verify that your Cisco FTD + FMC deployment is working correctly.  
The goal is to generate real traffic, visibility, and security events so you can confirm:

- NAT is working  
- ACP rules are matching  
- IPS is firing  
- URL filtering is operating  
- Connection and intrusion events are visible in FMC  

These tests simulate how a SOC or firewall engineer validates a deployment.

---

## 1. Basic Connectivity Tests (Test Client)

Start on the test VM behind FTD.

### Check default route:
```bash
ip route
Test basic ICMP (to confirm FTD forwarding):
bash
Copy code
ping 8.8.8.8
Test DNS resolution:
bash
Copy code
nslookup google.com
If DNS fails:

Check DNS object in FMC

Verify NAT rule

Confirm inside → outside ACP rule permits DNS

2. Validate NAT Translation
From the test client:

See your public IP translation:
bash
Copy code
curl ifconfig.me
Expected:

The response should show your FTD outside IP

If it shows your internal IP, NAT is not working.

3. Generate Web Traffic (Triggers Logs)
From the test client:

bash
Copy code
curl https://google.com
curl http://example.com
curl -I https://cloudflare.com
Browse websites to create more events.

Expected results in FMC:

Connections appear under Analysis → Connections

Applications identified (HTTP, SSL, DNS)

URL categories recorded

4. Trigger URL Filtering Events
Visit a category you blocked or monitored.

Example (if Gambling or Adult sites are blocked):

bash
Copy code
curl http://www.pokerstars.com
Expected in FMC:

URL Category shown

Action: Block/Monitor

Logged under:

mathematica
Copy code
Analysis → URL
5. Trigger IPS Events (Safe Tests)
You can safely trigger Snort test signatures.

Use the EICAR test string (harmless test file):
bash
Copy code
curl http://www.eicar.org/download/eicar.com
Expected:

FTD blocks the download

FMC logs:

File Event

Malware Event

IPS Signature Match

View under:

nginx
Copy code
Analysis → Intrusion
Analysis → Malware
Analysis → Files
6. Generate HTTP/HTTPS Traffic for Inspection
Try these:

bash
Copy code
curl http://example.com
curl -I https://github.com
wget https://speed.hetzner.de/100MB.bin
These generate:

File events

Application visibility

TLS handshake logs

Intrusion signatures (if present)

7. Check ACP Rule Matches
In FMC:

nginx
Copy code
Analysis → Connections
Sort by:

Rule Name

Action (Allow/Block)

Security Zones

NAT Policy

This confirms:

NAT rule used

ACP rule hit

URL rule applied

IPS policy used

8. Review Logs in FMC
Key log locations:

Connection Events
nginx
Copy code
Analysis → Connections
Shows:

Source/destination IP

Ports

URL category

Application

Rule hit

Intrusion Events
nginx
Copy code
Analysis → Intrusion → Events
Shows:

Snort signature hits

CVE mappings

Impact level

Severity

Source → destination

File & Malware Events
nginx
Copy code
Analysis → Files
Analysis → Malware
Triggered by:

File downloads

Malware signatures

HTTP/HTTPS inspection

URL Filtering
mathematica
Copy code
Analysis → URL
Shows:

URL categories

Block/monitor actions

Matching ACP rules

9. Packet Capture on FTD (Optional)
Use FTD CLI for deep troubleshooting:

Capture packets:
bash
Copy code
capture inside type raw-data interface inside match ip host <TEST-CLIENT-IP> any
capture outside type raw-data interface outside match ip any host 8.8.8.8
View packets:
bash
Copy code
show capture inside
show capture outside
Summary
By the end of this testing phase:

NAT should translate inside → outside

ACP rules should match correctly

IPS, URL, and Malware inspection should generate events

FMC should display all connection logs

Packet captures can confirm detailed behavior

Your Firepower deployment is now fully validated.