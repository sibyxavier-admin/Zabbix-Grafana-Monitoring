# 📊 Zabbix + Grafana
## Centralized IT Infrastructure Monitoring

![Zabbix](https://img.shields.io/badge/Zabbix-7.0-red)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-orange)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-black)
![SNMP](https://img.shields.io/badge/SNMP-v2%2Fv3-green)
![ICMP](https://img.shields.io/badge/ICMP-Availability-purple)
![Monitoring](https://img.shields.io/badge/Infrastructure-Monitoring-blue)

> 🚀 A production-grade centralized IT infrastructure monitoring platform built and deployed across a live campus network — providing real-time visibility, proactive alerting, and centralized dashboards.

---

## 📌 Project Overview

This project documents the end-to-end implementation of a centralized IT infrastructure monitoring and alerting platform for a multi-building campus network.

The platform was built and deployed in a real production environment to provide centralized visibility into server health, network availability, device performance, and infrastructure reliability.

### 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| 🐧 Ubuntu Linux | Monitoring Server |
| 🗄️ SQL Database | Zabbix Backend Database |
| 📡 Zabbix 7.0 | Infrastructure Monitoring |
| 🔌 Zabbix Agent | Server Monitoring |
| 📶 SNMP v2/v3 | Network Device Monitoring |
| 🔁 ICMP | Availability Monitoring |
| 📈 Grafana | Dashboard & Visualization |
| ✉️ SMTP | Automated Email Alerting |

### 📊 Production Monitoring Coverage

The monitoring platform covers:

- 🖥️ Windows & Linux Servers
- 🔀 L1/L2/L3 Netgear Switches
- 📡 Wireless Access Points
- 🖨️ Network Printers — Brother, HP, Ricoh, Konica Minolta
- 📹 Hikvision & Prama IP Cameras
- 🔥 Firewalls
- 🌐 Internet Uplinks & Load-Balancing Status

---

## 🎯 Objectives

- ✅ Centralize monitoring for multiple infrastructure device types
- ✅ Detect outages and performance issues proactively
- ✅ Monitor server and network availability in real time
- ✅ Visualize infrastructure health through centralized Grafana dashboards
- ✅ Reduce manual troubleshooting and response time
- ✅ Maintain historical performance data for trend analysis
- ✅ Provide IT administrators with a single centralized monitoring platform

---

## 🏗️ High-Level Architecture
```text
                    IT INFRASTRUCTURE
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
       ▼                   ▼                   ▼
    Servers          Network Devices      SNMP Devices
       │                   │                   │
       │               SNMP / ICMP             │
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                           ▼
                    ZABBIX SERVER
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
       SQL DATABASE               ZABBIX AGENT
              │                         │
              └────────────┬────────────┘
                           │
                           ▼
                      ZABBIX API
                           │
                           ▼
                        GRAFANA
                           │
                           ▼
                 CENTRALIZED DASHBOARD
                           │
                           ▼
                   IT ADMINISTRATOR
```
---
⚙️ Setup Guide
Step 1 — Prepare the Linux Server
Install a fresh Linux server (Ubuntu recommended) that will host Zabbix Server, the SQL database, and the web frontend.
Step 2 — Install SQL Database
Install and configure the database (MySQL/MariaDB) that Zabbix will use to store configuration and historical monitoring data.
Step 3 — Install Zabbix Server & Frontend
Install the Zabbix Server package, connect it to the SQL database, and set up the Zabbix web frontend for configuration and monitoring views.
Step 4 — Configure SMTP for Alerts
Set up email notifications so Zabbix can send alerts automatically when a problem is detected.
Step 5 — Install Zabbix Agent on Servers
Install the Zabbix Agent on every Linux/Windows server you want to monitor at the OS level.
```bash
sudo nano /etc/zabbix/zabbix_agentd.conf
```
Typical metrics collected:
CPU / Memory / Disk utilization
Disk I/O & network throughput
System uptime & running processes
Step 6 — Add Servers in Zabbix
In the Zabbix Web UI:
Data Collection → Hosts → Create Host
Enter hostname + select Host Group
Add the server IP under Agent Interface
Link the Zabbix Agent template
Click Add, then verify under Monitoring → Latest Data
Step 7 — Add Network Devices via SNMP
Used for switches, APs, printers, UPS, firewalls, and other SNMP-enabled gear.
Before adding a device, confirm:
SNMP is enabled on the device with the correct version (v2c/v3)
Zabbix Server can reach the device over UDP 161
Correct SNMP credentials are available
In Zabbix: `Data Collection → Hosts → Create Host` → set Host Name, Host Group, SNMP Interface, IP, Port 161, SNMP version/credentials → link the SNMP template.
> 🔒 **Security note:** Never commit real SNMP community strings or SNMPv3 credentials. Use placeholders like `<SNMP_COMMUNITY>`.
Step 8 — ICMP Ping Monitoring
For simple up/down reachability on devices where SNMP isn't available or needed (switches, printers, cameras, firewalls):
```text
Device Reachable   →  UP
Device Unreachable →  DOWN
```
Step 9 — Host Groups & Templates
Organize devices logically and reuse monitoring configs:
Example Host Groups: Linux Servers, Windows Servers, Network Switches, Wireless APs, Printers, Firewalls, CCTV Infrastructure
Example Template Stack per host:
```text
Host
 ├── ICMP Ping Template → Availability
 └── SNMP Network Device Template
        ├── CPU
        ├── Memory
        ├── Interface Traffic
        └── Port Status
```
Step 10 — Verify Monitoring
Data Collection → Hosts — confirm host is enabled and reachable
Monitoring → Latest Data — confirm values are populating
Monitoring → Problems — check for active issues
Confirm the correct template is linked on each host
Step 11 — Install Grafana
```bash
sudo systemctl status grafana-server
```
Follow the official Grafana docs for your distro to add the repo, install, and enable the service on boot.
Step 12 — Connect Grafana to Zabbix
```text
Zabbix Server → Zabbix API → Grafana Zabbix Data Source → Grafana Dashboard
```
Add the Zabbix data source plugin in Grafana and point it to your Zabbix API URL + credentials.
Step 13 — Build the Dashboard
Recommended panels:
```text
┌─────────────────────────────────────────────┐
│        CENTRALIZED IT MONITORING             │
├─────────────┬─────────────┬─────────────────┤
│ Hosts UP    │ Hosts DOWN  │ Active Problems  │
├─────────────┴─────────────┴─────────────────┤
│              Device Status                   │
├───────────────────────┬─────────────────────┤
│ CPU Utilization       │ Memory Utilization   │
├───────────────────────┴─────────────────────┤
│             Network Traffic                 │
├─────────────────────────────────────────────┤
│             Ping / Latency                  │
├─────────────────────────────────────────────┤
│             Historical Trends               │
└─────────────────────────────────────────────┘
```
Step 14 — Test & Validate
Confirm Zabbix Server, SQL, Agents, and Grafana are all running and data is flowing.
Simulate a failure on a test device and confirm the full alert chain works:
```text
Test Device → Unreachable → Zabbix Detects Failure → Trigger Fires → Problem Created → Alert Sent
```
---
🔒 Security Best Practices
Zabbix
Strong admin passwords + role-based access control
Separate accounts per admin, least-privilege permissions
HTTPS on the web interface, restrict access, keep Zabbix updated
SNMP
Prefer SNMPv3 where the device supports it
Never use default community strings
Restrict SNMP access to the Zabbix Server only; never expose UDP 161 publicly
Grafana
Strong credentials, HTTPS, role-based access
Protect API tokens/credentials, keep Grafana updated
Linux Server
Keep OS patched, use a firewall, disable unused services
SSH keys over passwords, restrict admin access, regular backups
Never commit:
Passwords · API tokens · SNMP community strings/SNMPv3 creds · SMTP passwords · DB passwords · SSH private keys · real infrastructure credentials
Use placeholders instead:
```text
<ZABBIX_SERVER_IP>
<ZABBIX_USERNAME>
<ZABBIX_PASSWORD>
<SNMP_COMMUNITY>
<GRAFANA_URL>
<SMTP_USERNAME>
<SMTP_PASSWORD>
```
---
✅ Result
A single Grafana dashboard now gives real-time visibility across the entire campus network — servers, switches, printers, and cameras — with automated email alerts firing the moment a device goes down. Troubleshooting that used to mean checking device-by-device now starts with one dashboard.
---
🙌 Credits
Built and documented by [Your Name] — IT Network & System Administrator.
Feel free to fork this repo and adapt the setup for your own infrastructure!
