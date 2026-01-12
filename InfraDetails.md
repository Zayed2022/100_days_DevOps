# 🏗️ Project Nautilus: Infrastructure Details

This repository documents my hands-on tasks for the **100 Days of DevOps Challenge**. All tasks are performed within the **Nautilus** infrastructure, a simulated enterprise environment provided by KodeKloud.

## 🏢 Datacenter: Stratos DC
The infrastructure is primarily hosted in the **Stratos Datacenter**, following a standard 3-tier architecture (Web, App, and Database).

---

## 🖥️ Server Inventory

### 1. Application Servers (Application Tier)
These servers run web applications, middleware, and backend services.

| Hostname | IP Address | OS | Role |
| :--- | :--- | :--- | :--- |
| **stapp01** | 172.16.238.10 | CentOS 7 | App Server 1 |
| **stapp02** | 172.16.238.11 | CentOS 7 | App Server 2 |
| **stapp03** | 172.16.238.12 | CentOS 7 | App Server 3 |

### 2. Database Server (Data Tier)
Hosts MariaDB/PostgreSQL databases for the applications.

| Hostname | IP Address | OS | Role |
| :--- | :--- | :--- | :--- |
| **stdb01** | 172.16.238.15 | CentOS 7 | Database Server |

### 3. Storage & Backup Servers
Dedicated servers for shared files and data archival.

| Hostname | IP Address | OS | Role |
| :--- | :--- | :--- | :--- |
| **ststor01** | 172.16.238.15 | CentOS 7 | Storage Server (NFS/NAS) |
| **stbr01** | 172.16.238.16 | CentOS 7 | Backup Server |

### 4. Load Balancer (Client Tier)
Manages incoming traffic and distributes it across the application servers.

| Hostname | IP Address | OS | Role |
| :--- | :--- | :--- | :--- |
| **stlb01** | 172.16.238.14 | CentOS 7 | LBR Server (Nginx) |

### 5. Management Server
The entry point for all administrative tasks.

| Hostname | IP Address | OS | Role |
| :--- | :--- | :--- | :--- |
| **jump_host** | 172.16.238.3 | CentOS 7 | Jump Server |

---

## 🔑 Infrastructure Credentials

| User | Password | Access Level |
| :--- | :--- | :--- |
| **tony** | `Ir0nM@n` | Sudo on `stapp01` |
| **steve** | `sTevE7` | Sudo on `stapp02` |
| **banner** | `BigGuy4u` | Sudo on `stapp03` |
| **thor** | `Odinson1` | Sudo on `jump_host` |
| **peter** | `Spidey3` | Sudo on `stdb01` |
| **natasha** | `BlackWidow` | Sudo on `ststor01` |
| **clint** | `H@wkeye` | Sudo on `stbr01` |
| **loki** | `GodOfMischief` | Sudo on `stlb01` |

---

## 🌐 Network Architecture
- **Jump Host:** Only server accessible from the outside. Used as a gateway to all other servers.
- **SSH Protocol:** Admin tasks are performed via SSH from the Jump Host to internal servers using the `sudo` user credentials listed above.
- **Standard Protocol:** Usernames are always created in **lowercase**.

  ---
# Project Nautilus

## OVERVIEW
Project Nautilus is run by the Naval subdivision within xFusionCorp Industries. The Nautilus Application helps the Naval forces to make smart procurement decisions on manned and unmanned maritime systems while ensuring that operational requirements are met. It aims to provide best in class operational support, improve the safety and life extension of existing machines, and reduce cost of ownership.

## Application Architecture
The Nautilus is a three-tier application and is deployed in the **Stratos Datacenter** in the North America Region.

* **Data Tier:** The Data tier is the layer that stores data with the retrieval storage and execution methods made by the application layer. We are making use of MariaDB which is one of the most popular open source relational databases.
* **Application Tier:** Makes use of a LAMP which is a stack of open-source software that can be used to create web applications. LAMP is an acronym that usually consists of the Linux OS, the Apache HTTP Server, a MySQL relational DBMS (like MariaDB), and PHP.
* **Client Tier:** The application client which in this case is a web browser software that processes and displays HTML resources, issues HTTP requests for resources, and processes HTTP responses.
* **Load Balancer:** Nginx is used for HTTP Load Balancing to distribute requests through multiple application servers.

## Shared Services
* **Storage Filer:** A NAS (Network Attached Storage) filer is used to provide reliable and stable external storage for the application tier servers.
* **SFTP Server:** SFTP, which stands for SSH File Transfer Protocol is used to transfer data amongst two remote systems.
* **Backup Server:** A staging backup system used for short term archival.
* **Jump Server:** The intermediary host or an SSH gateway to a remote network hosting the Nautilus application.

## Infrastructure Details

| Server Name | IP | Hostname | User | Password | Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **stapp01** | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM@n | Nautilus App 1 |
| **stapp02** | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Am3ric@ | Nautilus App 2 |
| **stapp03** | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | BigGr33n | Nautilus App 3 |
| **stlb01** | 172.16.238.14 | stlb01.stratos.xfusioncorp.com | loki | Mischi3f | Nautilus HTTP LBR |
| **stdb01** | 172.16.239.10 | stdb01.stratos.xfusioncorp.com | peter | Sp!dy | Nautilus DB Server |
| **ststor01** | 172.16.238.15 | ststor01.stratos.xfusioncorp.com | natasha | Bl@kW | Nautilus Storage Server |
| **stbkp01** | 172.16.238.16 | stbkp01.stratos.xfusioncorp.com | clint | H@wk3y3 | Nautilus Backup Server |
| **stmail01** | 172.16.238.17 | stmail01.stratos.xfusioncorp.com | groot | Gr00T123 | Nautilus Mail Server |
| **jump_host** | Dynamic | jump_host.stratos.xfusioncorp.com | thor | mjolnir123 | Jump Server |
| **jenkins** | 172.16.238.19 | jenkins.stratos.xfusioncorp.com | jenkins | j@rv!s | Jenkins Server for CI/CD |
---
*Reference: [KodeKloud Engineer - Project Nautilus](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus)*
