Based on the screenshot you provided, it looks like you ran into a specific issue with how the permissions were applied. Even though you ran `sudo chmod a+x /tmp/xfusioncorp.sh`, the final `ls -ltr` output shows the permissions as:

`---x--x--x 1 root root`

This means the script currently has **only** executable permissions, but **no read permissions** (`r`). Most shells require a script to be readable so they can interpret the code inside before executing it. This is likely why the validation failed with the message: *"'script' is not executable on App Server 3"*.

### **How to Fix the Task**

To make a script fully executable for everyone, it should typically have **Read** and **Execute** permissions. Use the numeric mode `755`, which is the standard for scripts:

```bash
sudo chmod 755 /tmp/xfusioncorp.sh

```

**Verification:**
After running that, run `ls -l /tmp/xfusioncorp.sh`. It should look like this:
`-rwxr-xr-x 1 root root`

---

### **Day 4: Updated README Notes**

# Day 4: Grant Executable Permissions to Scripts

## 📋 Task Overview
The xFusionCorp Industries sysadmin team developed a bash script `/tmp/xfusioncorp.sh` for backup processes. The task was to grant executable permissions to all users on **App Server 3** (`stapp03`).

### Task Specifications:
- **Server:** `stapp03`
- **File Path:** `/tmp/xfusioncorp.sh`
- **Requirement:** Executable by all users.

---

## 🚀 Implementation Steps

1. **Access Server:**
   SSH into `stapp03` as user `banner`.

2. **Grant Permissions:**
   Originally, using `chmod +x` only added the execute bit. To ensure the script runs correctly, it needs both read and execute permissions for all users.
   ```bash
   sudo chmod 755 /tmp/xfusioncorp.sh

```

3. **Verify Change:**

```bash
ls -l /tmp/xfusioncorp.sh

```


**Confirmed Output:** `-rwxr-xr-x`

---

## ⚠️ Common Pitfall: Execute vs. Read

While the task specifically asks for "executable permissions," in Linux, a script usually needs to be **readable** (`r`) by the user trying to **execute** (`x`) it.

* If permissions are set to `--x--x--x`, the shell cannot read the script's contents to execute them.
* Setting permissions to `755` (`rwxr-xr-x`) ensures the owner has full control, and everyone else can read and run the script safely.

---
