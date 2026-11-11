# Azure Sentinel Honeypot Lab

## Overview

This project is a hands-on security operations lab built in Microsoft Azure. A Windows virtual machine was deliberately exposed to the public internet to act as a honeypot. Failed RDP login attempts (Windows Security Event ID 4625) were collected into a Log Analytics workspace and analyzed in Microsoft Sentinel using KQL to identify attacking IP addresses, targeted usernames, attack timing, and geographic origin.

The goal of the lab was to practice real-world SOC skills: log ingestion, SIEM configuration, threat hunting with KQL, detection engineering, and visualizing attack data.

## Architecture

```
  Internet attackers
          |
          |  RDP brute force (TCP 3389)
          v
+---------------------------+
|  Azure Windows VM         |
|  (honeypot, open NSG)     |
|  Windows Security Log     |
|  Event ID 4625            |
+---------------------------+
          |
          |  Azure Monitor Agent / Data Collection Rule
          v
+---------------------------+
|  Log Analytics Workspace  |
|  SecurityEvent table      |
+---------------------------+
          |
          v
+---------------------------+
|  Microsoft Sentinel       |
|  KQL queries, workbook,   |
|  GeoIP watchlist, map     |
+---------------------------+
```

## Tools Used

- Microsoft Azure (Virtual Machines, Network Security Groups)
- Azure Log Analytics Workspace
- Microsoft Sentinel (SIEM)
- Kusto Query Language (KQL)
- Sentinel Watchlists (GeoIP lookup)
- Sentinel Workbooks (attack map visualization)
- Windows Event Viewer
- Remote Desktop Protocol (RDP)

## Setup Steps

1. **Create a resource group** in Azure to hold all lab resources.
2. **Deploy a Windows VM** and configure its Network Security Group to allow all inbound traffic so it is discoverable by attackers on the internet.
3. **Disable the Windows Firewall** inside the VM so connection attempts reach the system and generate logs.
4. **Create a Log Analytics workspace** to store collected logs.
5. **Enable Microsoft Sentinel** on the Log Analytics workspace.
6. **Connect the VM to the workspace** using the Windows Security Events data connector (Azure Monitor Agent with a Data Collection Rule) so Security Event logs, including Event ID 4625, are ingested.
7. **Upload a GeoIP watchlist** named `geoip` to Sentinel to map IP addresses to country, latitude, and longitude.
8. **Run the KQL queries** in the `queries/` folder to investigate failed logins.
9. **Build a Sentinel workbook** using the attack map query to visualize where attacks originate.
10. **Tear down resources** when finished to avoid ongoing costs and risk.

## KQL Queries

| Query | Description |
|-------|-------------|
| [failed-logins.kql](queries/failed-logins.kql) | Lists every failed login (Event ID 4625) with time, source IP, computer, account, and port. |
| [top-attacking-ips.kql](queries/top-attacking-ips.kql) | Shows the 10 source IP addresses with the most failed login attempts. |
| [top-usernames.kql](queries/top-usernames.kql) | Shows the 10 usernames attackers tried most often. |
| [attacks-over-time.kql](queries/attacks-over-time.kql) | Charts failed login attempts per hour as a time series. |
| [attack-map.kql](queries/attack-map.kql) | Enriches attacker IPs with GeoIP data to plot attack origins on a map. |
| [brute-force-detection.kql](queries/brute-force-detection.kql) | Flags any IP with more than 10 failed logins in a single hour as likely brute force. |

## Findings

- **Lab duration:** [ADD DURATION VM WAS EXPOSED]
- **Total failed login attempts:** [ADD TOTAL ATTEMPTS]
- **Unique attacking IP addresses:** [ADD UNIQUE IP COUNT]
- **Top attacking IP:** [ADD TOP IP AND ATTEMPT COUNT]
- **Top attacking countries:** [ADD TOP COUNTRIES]
- **Most targeted usernames:** [ADD TOP USERNAMES]
- **Time to first attack after exposure:** [ADD TIME TO FIRST ATTACK]
- **Peak attack period:** [ADD PEAK HOUR OR DAY]
- **IPs flagged by brute force detection:** [ADD COUNT OF FLAGGED IPS]

## Screenshots

> Screenshots are stored in the [`images/`](images/) folder.

- **Attack map:** [ADD SCREENSHOT: images/attack-map.png]
- **Failed logins query results:** [ADD SCREENSHOT: images/failed-logins.png]
- **Top attacking IPs:** [ADD SCREENSHOT: images/top-attacking-ips.png]
- **Top targeted usernames:** [ADD SCREENSHOT: images/top-usernames.png]
- **Attacks over time chart:** [ADD SCREENSHOT: images/attacks-over-time.png]
- **Brute force detection results:** [ADD SCREENSHOT: images/brute-force-detection.png]
- **Azure resource group overview:** [ADD SCREENSHOT: images/resource-group.png]

## Lessons Learned

- [ADD LESSON ABOUT HOW QUICKLY EXPOSED SYSTEMS ARE TARGETED]
- [ADD LESSON ABOUT COMMON USERNAMES ATTACKERS TRY]
- [ADD LESSON ABOUT WRITING AND TUNING KQL QUERIES]
- [ADD LESSON ABOUT CONFIGURING LOG INGESTION AND SENTINEL]
- [ADD LESSON ABOUT DETECTION THRESHOLDS AND FALSE POSITIVES]
- [ADD LESSON ABOUT CLOUD COST AND SECURITY HYGIENE]

---

**Disclaimer:** This lab intentionally exposed a VM to the internet for educational purposes in an isolated environment. Do not replicate this on production systems or networks.
