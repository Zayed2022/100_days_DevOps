# Day 3: Secure Root SSH Access

## 📋 Task Overview
The **xFusionCorp Industries** security team has mandated a new protocol to harden all application servers. The primary goal is to **disable direct SSH root login** on all app servers in the Stratos Datacenter. This forces users to log in with their own credentials and use `sudo` for administrative tasks, which improves security auditing.

### Task Specifications:
- **Target Servers:** `stapp01`, `stapp02`, `stapp03`
- **Configuration File:** `/etc/ssh/sshd_config`
- **Directive:** `PermitRootLogin no`

---

## 🛠️ Solution 1: Manual Editing (The Beginner Way)
This method involves opening the file manually and editing the text. 

1. **SSH into the server:**
   ```bash
   ssh tony@stapp01

```

2. **Open the configuration file:**
  ```bash
sudo vi /etc/ssh/sshd_config

```


3. **Edit the directive:**
* Use `/PermitRootLogin` to find the line.
* Press `i` to enter Insert Mode.
* Change `yes` to `no` (and remove the `#` if it is commented).
* Press `Esc`, then type `:wq!` and `Enter` to save and exit.


4. **Restart the SSH service:**
```bash
sudo systemctl restart sshd

```

**Do same to other servers as well**



---

## 🚀 Solution 2: Using `sed` (The DevOps Way)

This method is faster and minimizes human error. It uses the Stream Editor (`sed`) to modify the file without opening a visual editor.

1. **Run the `sed` command:**
The following command searches for any line starting with `#PermitRootLogin` or `PermitRootLogin` and replaces the entire line with `PermitRootLogin no`.
The sed Command for this Task
On each app server, run this command:

```bash

sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config
```
**Breakdown of the Command:**
sudo: Required because /etc/ssh/sshd_config is owned by the root user.

sed: The stream editor utility.

-i: Stands for "in-place". This tells sed to save the changes directly to the file instead of just printing the result to your terminal.

's/.../.../g': This is the "substitute" command:

s = Substitute

PermitRootLogin yes = The pattern to find.

PermitRootLogin no = The string to replace it with.

g = Global (replaces all occurrences, though usually there is only one in this file).

/etc/ssh/sshd_config: The path to the file you are modifying.<br>
**OR when # is in front then use**

```bash
sudo sed -i 's/^#*PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config

```


* `-i`: Edits the file in place.
* `s/pattern/replacement/`: Substitutes the pattern.
* `^#*`: Handles both commented and uncommented versions of the line.


2. **Restart the service:**
```bash
sudo systemctl restart sshd

```



---

## 🔍 Verification

After performing the steps on all servers (`stapp01`, `stapp02`, `stapp03`), test the restriction from the **Jump Host**:

```bash
# Attempt to login as root
ssh root@172.16.238.10

```

**Expected Result:**
`Permission denied (publickey,password).`
*(This confirms root login is disabled, even if the password is known.)*

---

## 🧠 Key Takeaways

* **Hardening:** Disabling root SSH is one of the most basic and important steps in server security.
* **Service Reloads:** Changing a configuration file in `/etc/` does nothing until the corresponding service (in this case `sshd`) is restarted or reloaded.
* **Automation:** Using tools like `sed` allows you to create scripts that can secure hundreds of servers simultaneously.

```
