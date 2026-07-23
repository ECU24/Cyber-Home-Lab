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















