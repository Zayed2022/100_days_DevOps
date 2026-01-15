# 🚀 100 Days of DevOps Challenge - Project Nautilus

Welcome to my **100 Days of DevOps** journey! In this repository, I document my daily progress solving real-world engineering tasks within the **xFusionCorp Industries** infrastructure, specifically focusing on **Project Nautilus**.

The challenge is based on the [KodeKloud Engineer](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus) platform, simulating a professional DevOps environment.

## 🏢 Infrastructure Overview
The project is set within the **Stratos Datacenter**, managing a three-tier application architecture.

| Component | Servers | Purpose |
| :--- | :--- | :--- |
| **App Tier** | `stapp01`, `stapp02`, `stapp03` | LAMP Stack Application Servers |
| **Load Balancer** | `stlb01` | Nginx HTTP Load Balancing |
| **Database** | `stdb01` | MariaDB Relational Database |
| **Storage** | `ststor01` | NAS Storage Filer |
| **Backup** | `stbkp01` | Short-term Archival |
| **Jump Host** | `jump_host` | SSH Gateway to the environment |

> [!TIP]
> For a full breakdown of IPs and hostnames, check out [InfraDetails.md](./InfraDetails.md).

## 📅 Daily Progress Log

| Day | Task Completed | Documented Solution |
| :--- | :--- | :--- |
| **Day 2** | DevOps Task Update | [View Solution](./Day2.md) |
| **Day 3** | Linux Task Progress | [View Solution](./Day3.md) |
| **Day 4** | Infrastructure Automation | [View Solution](./Day4.md) |
| **Day 5** | Server Configuration | [View Solution](./Day5.md) |
| **Day 6** | Service Management | [View Solution](./Day6.md) |
| **Day 7** | System Administration | [View Solution](./Day7.md) |
| **Day 8** | Network & Security | [View Solution](./Day8.md) |
| **Day 9** | Database Operations | [View Solution](./Day9.md) |
| **Day 10** | Storage Configuration | [View Solution](./Day10.md) |
| **Day 11** | Backup & Recovery | [View Solution](./Day11.md) |
| **Day 12** | Load Balancing | [View Solution](./Day12.md) |
| **Day 13** | CI/CD & Jenkins | [View Solution](./Day13.md) |

## 🛠 Skills & Tools Covered
- **Linux Administration:** User management, permissions, and system services.
- **Web Servers:** Configuring Apache and Nginx for high availability.
- **Database:** MariaDB/MySQL setup and maintenance.
- **Automation:** Shell Scripting and CI/CD via Jenkins.
- **Security:** SSH hardening, Firewall rules, and access control.

## 📂 Repository Structure
- Each `DayX.md` file contains the specific challenge description, the commands executed, and the final solution details.
- `InfraDetails.md` acts as the quick-reference guide for the entire Nautilus server farm.

---
*Stay tuned as I continue to complete the remaining tasks in this 100-day journey!*
