# Day 6: Cron Job Installation and Scheduling

## 📘 Task Overview
The Nautilus system administration team plans to deploy automation scripts across all application servers in the Stratos Datacenter.  
Before deploying production scripts, a **sample cron job** was configured to validate scheduling and execution.

---

## 🧾 Task Requirements
Perform the following steps on **all Nautilus App Servers**:

1. Install the `cronie` package
2. Start and enable the `crond` service
3. Add a cron job for the **root user**:
```bash

*/5 * * * * echo hello > /tmp/cron_text

```

---

## 🖥️ Server Details
- **Servers:**
- `stapp01`
- `stapp02`
- `stapp03`
- **OS Type:** RHEL/CentOS-based Linux
- **User:** `tony`
- **Cron User:** `root`

---

## ⚙️ Solution Steps

> The following steps were executed on **each app server**.

---

### 🔹 Step 1: Login to the Server
```bash
ssh tony@stapp01
````

(Repeated for `stapp02` and `stapp03`)

---

### 🔹 Step 2: Install cronie Package

```bash
sudo yum install -y cronie
```

The `cronie` package provides the cron scheduling utility.

---

### 🔹 Step 3: Start and Enable crond Service

```bash
sudo systemctl start crond
sudo systemctl enable crond
```

This ensures the cron daemon is running and persists after reboot.

---

### 🔹 Step 4: Configure Root Cron Job

Edit the root user’s crontab:

```bash
sudo crontab -e
```

Add the following entry:

```cron
*/5 * * * * echo hello > /tmp/cron_text
```

This job runs every 5 minutes and writes `hello` to `/tmp/cron_text`.

---

## ✅ Verification (Optional)

```bash
sudo crontab -l
cat /tmp/cron_text
```

Expected output:

```
hello
```

---

## 🏁 Final Outcome

* Cron service installed and running on all app servers
* Root cron job configured successfully
* Scheduled task executes every 5 minutes as expected

---

## 📚 Learning Outcome

* Understood cron scheduling syntax
* Learned difference between `cronie` package and `crond` service
* Configured system-wide automation using root cron jobs

---
