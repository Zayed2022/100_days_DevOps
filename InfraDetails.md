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
*Reference: [KodeKloud Engineer - Project Nautilus](https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus)*
