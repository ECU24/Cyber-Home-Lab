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
<img src="https://github.com/ECU24/Cyber-Home-Lab/blob/c4e93afc619923618a8fb7ac90a099c1354ce4e1/SOCLAB-diagram.png">
<br />
<br />
