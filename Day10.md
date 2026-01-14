🟢 **Task Name**
Day 10: Website Backup Automation using Bash 📦🖥️

---

📌 **Task Details (As Given)**

The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 2 in Stratos Datacenter, and they need to create a bash script named media_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 2).

a. Create a zip archive named xfusioncorp_media.zip of /var/www/html/media directory.

b. Save the archive in /backup/ on App Server 2. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in /backup/ location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.

e. Do not use sudo inside the script.

Note:
The zip package must be installed on given App Server before executing the script. This package is essential for creating the zip archive of the website files. Install it manually outside the script.

---

🤔 **WHAT is being asked here?**

We need to:

* Write a **bash script**
* Take a **backup of website media files**
* Compress them into a **zip file**
* Store the backup locally
* Copy it to the **backup server**
* Ensure the script:

  * Runs without password
  * Does not use sudo
  * Can be run by the server user

---

🤔 **WHY is this required?**

* Websites contain important static files (images, videos, media)
* Data loss can happen due to:

  * Server crash
  * Accidental deletion
  * Deployment mistakes
* Automated backups:

  * Save time
  * Reduce human error
  * Are industry best practice

This is a **real DevOps automation scenario**.

---

⏰ **WHEN is this used in real life?**

* Nightly website backups
* Before application deployments
* Disaster recovery planning
* Compliance and audit requirements

---

🧠 **HOW will the solution work? (High-level)**

1️⃣ Compress website media directory
2️⃣ Save zip file locally on App Server 2
3️⃣ Copy zip file to Backup Server
4️⃣ Use SSH key-based authentication
5️⃣ Run script without sudo

---

🖥️ **Infrastructure Context (Important)**

App Server 2

* Hostname: `stapp02`
* User: `steve`
* Website path: `/var/www/html/media`
* Script location: `/scripts`

Backup Server

* Hostname: `stbkp01`
* User: `clint`
* Backup path: `/backup`

---

⚠️ **Pre-Requisites (Must Be Done First)**

---

🔹 **Install zip package (outside the script)**

WHY:
Task explicitly says zip must be installed manually.

```bash
ssh steve@stapp02
sudo yum install -y zip
```

---

🔹 **Configure Password-less SSH to Backup Server**

WHY:
Script must not ask for password.

From **App Server 2**:

```bash
ssh-keygen -t rsa
ssh-copy-id clint@stbkp01
```

Test:

```bash
ssh clint@stbkp01
```

If no password is asked → ready ✅

---

⚙️ **HOW the Script Was Created**

---

🔹 **Step 1: Create scripts directory**

```bash
mkdir -p /scripts
```

---

🔹 **Step 2: Create the script file**

```bash
vi /scripts/media_backup.sh
```

---

🔹 **Step 3: Script Content**

```bash
#!/bin/bash

SOURCE_DIR="/var/www/html/media"
BACKUP_DIR="/backup"
BACKUP_FILE="xfusioncorp_media.zip"
BACKUP_SERVER="stbkp01"
BACKUP_USER="clint"
REMOTE_BACKUP_DIR="/backup"

# Create zip archive
zip -r ${BACKUP_DIR}/${BACKUP_FILE} ${SOURCE_DIR}

# Copy archive to backup server
scp ${BACKUP_DIR}/${BACKUP_FILE} ${BACKUP_USER}@${BACKUP_SERVER}:${REMOTE_BACKUP_DIR}
```

---

🔹 **Step 4: Make Script Executable**

```bash
chmod +x /scripts/media_backup.sh
```

---

▶️ **HOW to Run the Script**

Run as normal user (no sudo):

```bash
/scripts/media_backup.sh
```

---

✅ **HOW to Verify Backup**

On App Server 2:

```bash
ls -l /backup/xfusioncorp_media.zip
```

On Backup Server:

```bash
ssh clint@stbkp01
ls -l /backup/xfusioncorp_media.zip
```

If file exists on both servers → success 🎉

---

🚫 **WHY sudo is NOT used in the script**

* Security best practice
* Scripts should follow least privilege
* sudo inside scripts can break cron jobs
* Task requirement strictly forbids it

---

🧠 **BEGINNER KEY TAKEAWAYS**

* Bash scripts automate repetitive tasks
* zip is used for compression
* scp copies files securely over SSH
* SSH keys are mandatory for automation
* Variables make scripts clean and reusable

---

⚠️ **COMMON MISTAKES TO AVOID**

❌ Using sudo inside script
❌ Forgetting to install zip
❌ No SSH key setup
❌ Wrong user or backup path
❌ Script not executable

---

🎉 **FINAL STATUS**

✔ Script created successfully
✔ Website media backed up
✔ Zip archive created
✔ Backup copied to backup server
✔ No password prompt
✔ Task completed as per requirements

---

