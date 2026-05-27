Policies → Access Control → Access Control Policies → New Policy


Recommended settings:

- **Name:** Enterprise-ACP  
- **Base Policy:** Balanced Security and Connectivity  
- **Inspection Mode:** Access Control + IPS  

This gives a strong, realistic baseline.

---

## 2. Define Security Zones

Zones must match the FTD interfaces.

In FMC:



Objects → Object Management → Interface → Security Zones


Create:

- `INSIDE_ZONE`
- `OUTSIDE_ZONE`

Assign interfaces accordingly:

- Inside interface → INSIDE_ZONE  
- Outside interface → OUTSIDE_ZONE  

Zones allow ACP rules to be directional (inside → outside).

---

## 3. Build Core ACP Rules

Below are recommended rules for a clean, enterprise-style policy.

---

### ✔ Rule 1 — Allow Inside → Outside (General Web Traffic)

**Purpose:** Allow inside clients to reach the internet while applying inspection.

Settings:

- **Source Zone:** INSIDE_ZONE  
- **Destination Zone:** OUTSIDE_ZONE  
- **Source Network:** `<INSIDE-SUBNET>`  
- **Applications:** Any  
- **URL Filtering:** Monitor  
- **IPS Policy:** Balanced Security and Connectivity  
- **Logging:** Log at End of Connection  

This rule generates:

- Connection events  
- Intrusion events (if triggered)  
- URL category logs  
- File/Malware analysis logs  

---

### ✔ Rule 2 — Block High-Risk Applications

Examples:

- BitTorrent  
- TOR  
- Proxy tools  
- Peer-to-peer clients  

Use FMC search:



Applications → Search “High Risk”


Action:

- **Block**
- **Log at Beginning and End**

This demonstrates application visibility & control (AVC).

---

### ✔ Rule 3 — URL Filtering (Category-Based)

Examples:

- Adult Content  
- Gambling  
- Malware Sites  
- Dynamic DNS  

Settings:

- **Action:** Block or Monitor  
- **Logging:** Enabled  

This shows understanding of Firepower URL filtering.

---

### ✔ Rule 4 — IPS-Focused Rule (Optional)

Targets specific traffic for deeper analysis.

Example:

- **Source Zone:** INSIDE_ZONE  
- **Destination Zone:** OUTSIDE_ZONE  
- **Application:** Web Browsing  
- **IPS Policy:** Balanced or Security Over Connectivity  
- **Action:** Allow with inspection  

IPS generates valuable intrusion events for your lab.

---

## 4. Set the Default Action

The default action applies when no rules match.

Recommended:

- **Action:** Block All Traffic  
- **Intrusion Policy:** Balanced Security and Connectivity  

This matches real enterprise best practices.

---

## 5. Save & Deploy Policy

Every change in FMC must be deployed to take effect.



Deploy → Deployment → Deploy


Make sure the deployment completes successfully.

---

## Summary

After completing ACP setup:

- Inside hosts can browse the web through the firewall  
- High-risk traffic (apps/sites) is blocked  
- IPS, URL filtering, and Malware/File analysis are active  
- All activity generates logs in FMC  
- Default deny rule protects the network  