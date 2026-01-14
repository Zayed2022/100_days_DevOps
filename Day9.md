🟢 **Task Name**
Day 9: Fix MariaDB Service Issue 🚨🛢️

---

📌 **WHAT was the issue?**

The Nautilus application in **Stratos Datacenter** was **unable to connect to the database**.
This caused a **production issue**, meaning the application could not function properly.

After investigation, it was found that:
👉 The **MariaDB service was DOWN** on the **database server**.

---

🤔 **WHY is this a critical problem?**

* The application depends on the database for:

  * Reading data
  * Writing data
  * Core functionality
* If the database is down:

  * Application fails
  * Users face errors
  * Business impact occurs

This is treated as a **critical / P1 production incident** 🚨.

---

⏰ **WHEN do such issues happen in real life?**

* After permission changes
* After backup/restore activities
* After server reboot
* After manual file operations
* After security hardening (SELinux / ownership changes)

These issues are **very common in production environments**.

---

🧠 **HOW does the application connect to the database? (Simple)**

Flow:

* Application Servers ➝ connect to ➝ MariaDB
* MariaDB runs as a **Linux service**
* Service must be **running and writable**

If MariaDB is stopped → no DB connections possible.

---

🖥️ **WHERE was the issue? (Infra Context)**

Database Server:

* Hostname: `stdb01.stratos.xfusioncorp.com`
* User: `peter`
* Database Service: `mariadb`
* Data Directory: `/var/lib/mysql`

---

⚙️ **HOW the Issue Was Investigated**

---

🔹 **Step 1: Login to Database Server**

WHY:
The issue was identified on the DB server.

```bash
ssh peter@stdb01
```

---

🔹 **Step 2: Check MariaDB Service Status**

WHY:
Always confirm service state before fixing.

```bash
sudo systemctl status mariadb
```

Result:

* Service was **inactive (dead)**
* Status showed: *MariaDB server is down*

---

🔹 **Step 3: Check Logs for Root Cause**

WHY:
Logs tell *why* a service failed.

```bash
sudo journalctl -u mariadb --no-pager | tail -20
```

Key error found:

```
Errcode: 13 "Permission denied"
```

---

🧠 **WHAT does “Permission denied” mean here?**

* MariaDB tried to write to its data directory
* Linux blocked the operation
* Service stopped immediately

This usually points to **wrong file ownership or permissions**.

---

🔍 **Step 4: Check Data Directory Ownership**

WHY:
MariaDB runs as user **mysql**, so it must own its data files.

```bash
ls -ld /var/lib/mysql
```

Output showed:

* Owner: `root`
* Group: `mysql`

❌ This is incorrect for MariaDB.

---

⚙️ **HOW the Issue Was Fixed**

---

🔹 **Step 5: Fix Ownership of MySQL Data Directory**

WHY:
MariaDB must fully own its data directory.

```bash
sudo chown -R mysql:mysql /var/lib/mysql
```

This allowed:

* File creation
* Data writing
* Normal DB operations

---

🔹 **Step 6: Enable MariaDB Service**

WHY:
Ensure service starts automatically after reboot.

```bash
sudo systemctl enable mariadb
```

---

🔹 **Step 7: Start MariaDB Service**

WHY:
Enable does not start the service immediately.

```bash
sudo systemctl start mariadb
```

---

🔹 **Step 8: Verify Service Is Running**

WHY:
Never assume — always verify.

```bash
sudo systemctl status mariadb
```

Result:

* `Active: active (running)`
* Status: *Taking your SQL requests now...*

✅ Service restored successfully.

---

🎉 **FINAL RESULT (WHAT Changed?)**

✔ MariaDB service is running
✔ Permission issue resolved
✔ Database server is healthy
✔ Application can connect to database
✔ Production issue fixed

---

🧠 **BEGINNER KEY TAKEAWAYS (VERY IMPORTANT)**

* Services run as specific users (not root)
* Databases need write access to data directories
* `Errcode: 13` almost always means permission issues
* Logs are your best debugging tool
* `enable` and `start` are different commands

---

⚠️ **COMMON MISTAKES TO AVOID**

❌ Reinstalling MariaDB unnecessarily
❌ Deleting `/var/lib/mysql`
❌ Ignoring logs
❌ Running services as root
❌ Forgetting to verify after fix

---

📝 **REAL-WORLD INCIDENT SUMMARY (How to explain in job)**

> The MariaDB service failed to start due to incorrect ownership of the database data directory. Restoring proper ownership to `/var/lib/mysql` and restarting the service resolved the issue and restored application connectivity.

---
