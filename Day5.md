# Day 5: SELinux Installation and Configuration

## 📘 Task Overview
Following a security audit, the xFusionCorp Industries security team decided to enhance application and server security using **SELinux**.  
As part of initial testing, SELinux needed to be installed and **permanently disabled** on **App Server 2** in the Stratos Datacenter.

> Note: The server will be rebooted later during scheduled maintenance, so no reboot was required while performing this task.

---

## 🧾 Task Requirements
- Install the required SELinux packages
- Permanently disable SELinux
- Do **not** reboot the server
- Ignore the current SELinux runtime status
- Ensure SELinux is disabled **after the next reboot**

---

## 🖥️ Server Details
- **Server Name:** App Server 2
- **Hostname:** `stapp02`
- **OS Type:** RHEL/CentOS-based Linux
- **User:** `steve`

---

## ⚙️ Solution Steps

### 🔹 Step 1: Login to App Server 2
```bash
ssh steve@stapp02
```

---

### 🔹 Step 2: Install SELinux Packages

The required SELinux-related packages were installed using the YUM package manager:

```bash
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils
```

This ensures that SELinux policies and management utilities are available on the server.

---

### 🔹 Step 3: Permanently Disable SELinux

To permanently disable SELinux, the configuration file was updated:

```bash
sudo vi /etc/selinux/config
```

The following change was made:

```ini
SELINUX=disabled
SELINUXTYPE=targeted
```

This ensures that SELinux will be **disabled after the next reboot**.

---

## 🚫 Important Notes

* No reboot was performed as per task instructions
* The `setenforce` command was not used since it only changes runtime behavior
* SELinux status verification was intentionally skipped

---

## ✅ Final Outcome

* Required SELinux packages installed successfully
* SELinux permanently disabled via configuration file
* System ready for scheduled reboot where changes will take effect

---
