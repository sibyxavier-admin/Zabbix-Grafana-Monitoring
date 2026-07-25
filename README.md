# Zabbix + Grafana Centralized Monitoring

A complete technical guide for implementing a centralized IT infrastructure monitoring platform using Linux Server, SQL Database, Zabbix, SNMP, ICMP, Zabbix Agent, Grafana, and automated alerting.

## 📌 Project Overview

This project demonstrates the implementation of a centralized monitoring solution designed to provide real-time visibility into IT infrastructure, proactive issue detection, faster troubleshooting, and improved infrastructure reliability.

The monitoring platform integrates:

- Linux Server
- SQL Database
- Zabbix Server
- Zabbix Agent
- SNMP Monitoring
- ICMP Ping Monitoring
- Grafana
- Zabbix-Grafana Integration
- Automated Alerting
- Email Notifications
- Centralized Monitoring Dashboards

---

## 🏗️ Monitoring Architecture

```text
                    IT INFRASTRUCTURE
                           │
          ┌────────────────┼────────────────┐
          │                │                │
        SNMP             ICMP         Zabbix Agent
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                   ┌───────────────┐
                   │ ZABBIX SERVER │
                   │               │
                   │ Hosts         │
                   │ Items         │
                   │ Triggers      │
                   │ Templates     │
                   │ Alerts        │
                   └───────┬───────┘
                           │
                           ▼
                   ┌───────────────┐
                   │ SQL DATABASE  │
                   │               │
                   │ History       │
                   │ Trends        │
                   │ Events        │
                   │ Configuration │
                   └───────┬───────┘
                           │
                           ▼
                   ┌───────────────┐
                   │    GRAFANA    │
                   │               │
                   │ Dashboards    │
                   │ Graphs        │
                   │ Visualization │
                   └───────────────┘
