# Cloud SOC Home Lab (Azure + Microsoft Sentinel)

<h2>📌 Overview</h2>
This file documents a Security Operations Centre (SOC) home lab I built in Microsoft Azure using Microsoft Sentinel as the SIEM. The goal was to deploy an intentionally exposed virtual machine, capture real-world attack data from the internet, along with geo-IP data and visualise attacker activity on a live map. 

<h2>🎯 Objectives</h2>

- Set up a cloud-based SOC environment using Azure infrastructure
- Deploy an exposed VM (honeypot) to attract real attack traffic
- Transfer security logs into Microsoft Sentinel via a Log Analytics Workspace (Log Repository)
- Query the Log Analytics Workspace with KQL to filter data 
- Enrich failed/successful logon events with geolocation data using a watchlist
- Build a workbook to visualise attacker origins on a world map

<h2>🏗️ Architecture</h2>
<p align="center">
Architecture Diagram: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/7d3fc027bc8a83732672fea414de228953830b41/SOCLAB-diagram.png">
<br />
<br />

<h2>🛠️ Tools & Technologies</h2>

<div align="center">
  
| Category | Tool |
|---|---|
| Cloud Provider | Microsoft Azure |
| SIEM | Microsoft Sentinel |
| Log Storage | Log Analytics Workspace |
| Query Language | KQL (Kusto Query Language) |
| Import data | Sentinel Watchlist (Geo-IP dataset) |
| Visualisation | Sentinel Workbooks |
| Target VM | Windows 10/Server (RDP exposed) |

</div>
<br />

<h2>🚀 Project Walkthrough</h2>

<h3>1. Environment Setup </h3>
<p align="center">
Resource Group: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/ba7c87c7e944397e66a7ad50ce4e9d9f8a6fc23a/Resource-Group-Screenshot.png">

- Created a Resource Group (EU-SOC-LAB) and Virtual Network (Vnet-SO-LAB)
- Deployed a Windows VM with RDP exposed to the public internet
- Disabled the built-in firewall/real-time protection to allow unfiltered attack traffic for observation 
- Verified access to the VM through the public network using ping
<br />

<h3>2. Log Ingestion</h3>
<p align="center">
Log Analytics Workspace: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/df64c159c077478ebc1b1808cdefb30d248319ba/Log%20Analytics%20Workspace.png">

- Connected the VM to Log Analytics Workspace
- Enabled Security Event log collection 
- Viewed raw logs on the VM 
- Verified logon/logoff and failed authentication events were flowing into Sentinel
<br />

<h3>3. Geographical Internet Protocol Enrichment</h3>
<p align="center">
Geo-IP CSV Dataset: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/225d68af53d7c5d2de0862c051f4c40733fd6420/geo-IPDataset.png">

- Imported a geo-IP CSV dataset (55,000 rows) as a Sentinel Watchlist

<p align="center">
KQL Query to resolve attacker location: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/225d68af53d7c5d2de0862c051f4c40733fd6420/KQL-query.png">

- Used KQL to join failed/successful logon events against the watchlist to resolve attacker IPs to city/country/lat-long
<br />

<h3>4. Attack Map Workbook</h3>
<p align="center">
Attack Map: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/c9a81d2debe9cb2f85426b0e8cee277b7baa7722/AttackMap.png">

- The [JSON Code](https://github.com/ECU24/Cyber-Home-Lab/blob/c9a81d2debe9cb2f85426b0e8cee277b7baa7722/map.json) code used to create the map
- Built a Sentinel Workbook using the uploaded geo-IP data
- Visualised real-time attack origins on a world map, along with frequency counts per country
<br />

<h3>5. Hardening & Results</h3>
<p align="center">
Security hardening rules: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/c8eec903269fed082c9f12a2a0a7609e30a50b62/SecurityRule.png">

- Restricted the NSG to allow RDP only from my own Public IP address
- Removed the original "Allow RDP from Any" rule so it could no longer take precedence.
- Re-ran queries to compare attack volume before vs. after hardening
<br />

<h2>📊 Results</h2>

<div align="center">

| Metric | Before Hardening | After Hardening |
|---|---|---|
| Failed login attempts (24h) | 19,177 | 0 |
| Unique attacking IPs | 12 | 0 |
| Countries observed | 9 | 0 |

</div>
<br />

<p align="center">
Query showing results before: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/2886ddaad04bf8c396e5f423be38b573074e04ce/Before%20Hardening%20Query.png">

- Query used to output total failed attempts, unique attack IPs, and countries observed 24 hours before security hardening
<br />

<p align="center">
Query showing results after: <br/>
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/2886ddaad04bf8c396e5f423be38b573074e04ce/After%20Hardening%20Query.png">

- Query used to output total failed attempts, unique attack IPs, and countries observed 24 hours after security hardening
<br />

<h2>🧠 Lessons Learned</h2>

<h3>What surprised me about the volume/origin of attacks?</h3>
I wasn't expecting so many failed login attempts in such a short window; all of this happened in just 24 hours. It really shows how quickly threat actors can locate a vulnerable system, sometimes within a matter of hours, and this was only a honeypot with no real data on it. It really puts into perspective what these threat actors could accomplish against a real production environment with actual sensitive data and business-critical systems on the line

<h3>What would you configure differently in a production environment?</h3>
I would implement more sophisticated firewall and NSG rules to filter out suspicious activity before it ever reaches the host, features like geo-blocking unlikely regions, rate-limiting repeated failed attempts, and layering in a bastion host or VPN rather than exposing RDP directly to the internet.

























