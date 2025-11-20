# 🚀 Enterprise Network Automation & Monitoring Lab

Terraform · Proxmox · Ansible · Cisco · pfSense · FreeRADIUS · ELK · Zabbix

![GitHub last commit](https://img.shields.io/github/last-commit/yourusername/enterprise-network-lab)
![GitHub repo size](https://img.shields.io/github/repo-size/yourusername/enterprise-network-lab)
![GitHub issues](https://img.shields.io/github/issues/yourusername/enterprise-network-lab)
![GitHub pull requests](https://img.shields.io/github/issues-pr/yourusername/enterprise-network-lab)
![GitHub license](https://img.shields.io/github/license/yourusername/enterprise-network-lab)
![Ansible](https://img.shields.io/badge/automation-ansible-blue)
![Terraform](https://img.shields.io/badge/IaC-terraform-623CE4)
![Proxmox](https://img.shields.io/badge/virtualization-proxmox-orange)
![Cisco](https://img.shields.io/badge/networking-cisco-green)
![pfSense](https://img.shields.io/badge/firewall-pfSense-darkblue)


This project is a fully automated enterprise network environment that demonstrates infrastructure-as-code (IaC), network automation, monitoring, logging, and authentication workflows.

It is designed as a portfolio-grade project showcasing senior-level skills across networking, DevOps, automation, and security.

## 📡 1. Project Overview

This lab simulates an enterprise environment including:

- pfSense firewall/router

- Cisco Layer-3 switch with VLANs

- FreeRADIUS authentication for network access (802.1X + MAC auth)

- ELK Stack (ElasticSearch, Logstash, Kibana) for logs

- Filebeat agents on all hosts

- Zabbix for network monitoring

- Terraform + Proxmox for VM provisioning

- Ansible for network & server automation

## 🏢 2. About This Project

This repository contains a fully documented **Enterprise Networking & Monitoring Lab**, built to simulate a real-world corporate environment.  
It combines **firewalling, switching, virtualization, monitoring, logging, and automation** into one cohesive engineering project suitable for professional portfolios.

The goal of this project is to demonstrate end-to-end capability across:

- **Enterprise network design** (VLANs, routing, segmentation, Zero Trust principles)
- **pfSense firewall configuration** (NAT, firewall rules, VPN)
- **Cisco Catalyst switching** (VLANs, trunks, STP, RADIUS)
- **Infrastructure automation** using:
  - **Terraform** for provisioning a Proxmox virtualized environment  
  - **Ansible** for pushing Cisco configs, configuring ELK, Filebeat, Zabbix, and FreeRADIUS
- **Monitoring architecture**
  - Zabbix for metrics and alerts  
  - Grafana dashboards  
- **Centralized logging stack**
  - ELK (Elasticsearch, Logstash, Kibana)  
  - Filebeat clients
- **Security best practices**
  - Network ACLs  
  - RADIUS authentication  
  - Syslog and audit trails  
  - Device hardening templates

This project mirrors what a mid-size enterprise would deploy — making it ideal for interviews, portfolio demonstrations, or skill validation for roles in:

- Network Engineering  
- Systems Administration  
- DevOps / Infrastructure  
- Cybersecurity  
- SRE / Platform Operations  

All configurations, automation playbooks, and documentation are fully reproducible.


## 🏗 3. Lab Architecture

```
                   +-----------------------+
                   |       Proxmox         |
                   | (Terraform Provision) |
                   +-----------+-----------+
                               |
        ----------------------------------------------------
        |                         |                        |
+---------------+       +----------------+        +------------------+
| pfSense FW    |       | Ubuntu ELK     |        | Win Server AD    |
| VLAN Routing  |       | Zabbix Server  |        | DHCP / DNS       |
+-------+-------+       +--------+-------+        +---------+--------+
        |                        |                          |
        |               +--------+-------+                  |
      trunk             |  FreeRADIUS    |                  |
        |               +----------------+                  |
+-------+-------- Cisco Core Switch -------------------------+
| VLAN10 Mgmt | VLAN20 Servers | VLAN30 Staff | VLAN40 Guest | VLAN50 Clinical | VLAN70 IoT |
+-------------+----------------+--------------+---------------+-----------------+-------------+
```

## 📁 4. Repository Structure

```
enterprise-network-lab/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── diagrams/
│   ├── network-topology.png
│   ├── vlan-map.png
│   └── firewall-flow.png
│
├── docs/
│   ├── vlan-plan.md
│   ├── network-policies.md
│   ├── deployment-guide.md
│   └── incident-scenario.md
│
├── configs/
│   ├── pfsense-backup.xml
│   ├── cisco-startup-config.txt
│   └── switch-port-mapping.csv
│
├── ansible/
│   ├── site.yml
│   ├── cisco.yml
│   ├── freeradius.yml
│   ├── elk.yml
│   ├── zabbix.yml
│   ├── agent.yml
│   ├── inventory.ini
│   ├── group_vars/
│   ├── templates/
│   ├── roles/
│   └── backups/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfvars (ignored)
│   └── cloud-init/ubuntu-user-data.yaml
│
├── monitoring/
│   ├── zabbix-template.xml
│   ├── grafana-dashboard.json
│   ├── filebeat.yml
│   └── logstash.conf.j2
│
├── scripts/
│   ├── network-health-check.sh
│   ├── apply-infra.sh
│   └── backup-configs.sh
│
└── automation/
    ├── Makefile
    ├── pipeline.md
    └── ci-checks.yml
```

## 🧱 5. VLAN & Subnet Design

```
| VLAN | Purpose               | Subnet        |
| ---- | --------------------- | ------------- |
| 10   | Management            | 10.10.10.0/24 |
| 20   | Servers               | 10.10.20.0/24 |
| 30   | Staff Workstations    | 10.10.30.0/24 |
| 40   | Guest WiFi            | 10.10.40.0/24 |
| 50   | Clinical Systems      | 10.10.50.0/24 |
| 60   | Telemetry             | 10.10.60.0/24 |
| 70   | IoT / Medical Devices | 10.10.70.0/24 |
| 80   | DMZ                   | 10.10.80.0/24 |
```

## 6. Deployment Steps

### Step 1 — Provision Virtual Machines with Terraform

```
cd terraform
terraform init
terraform plan
terraform apply -auto-approve
```

This automatically deploys:

- pfSense

- Ubuntu server (ELK + Zabbix + Radius)

- Windows Server AD


### Step 2 — Run Ansible Master Playbook

```
ansible-playbook -i ansible/inventory.ini ansible/site.yml
```

This will:

- Push Cisco VLANs & trunk configs

- Install & configure FreeRADIUS

- Install ELK stack

- Install Zabbix server

- Install Filebeat & Zabbix on all VMs

- Run final health checks


## 🛠 7. Key Automations

**✔  FreeRADIUS Automation

- 802.1X users

- MAC authentication for IoT

- VLAN assignment (Tunnel attributes)

- Automated client definitions

**✔  ELK Stack Automation

- Elasticsearch install & tuning

- Logstash pipeline (template-driven)

- Kibana setup

- Filebeat deployment on all hosts

**✔  Zabbix Automation

- Server installation

- Database configuration

- Agents deployed via Ansible role


## 🧪 8. Monitoring & Health Checks

Script included:

```
scripts/network-health-check.sh
```

Performs:

- Ping tests across all VLANs

- API test to Zabbix server

- Logstash port availability

- FreeRADIUS authentication test

- Switch SNMP reachability


## 📚 9. Documentation

Inside docs/:

- deployment-guide.md
Full install instructions (pfSense → Cisco → ELK → Zabbix → RADIUS).

- vlan-plan.md
Detailed VLAN strategy.

- incident-scenario.md
A real-world failure simulation for interviews.

- network-policies.md
Enterprise security & segmentation policy.


## 🎯 10. Purpose of This Project

This repository demonstrates:

- Network engineering (VLAN, routing, firewalling)

- DevOps automation (Terraform, Ansible)

- Monitoring & observability (Zabbix, ELK)

- Authentication security (802.1X, RADIUS)

- CI/CD practices

- Documentation excellence

It is designed to be a flagship portfolio project and a strong demonstration of enterprise ICT expertise.
