🟢 **Task Name**
Day 8: Ansible Installation on Jump Host ⚙️🤖

---

📌 **WHAT is the task?**

The task is to **install Ansible version 4.7.0** on the **jump host**, which will act as the **Ansible controller**.

Key conditions:

* Installation must be done using **pip3 only**
* Ansible must be available **globally**
* All users on the jump host should be able to run Ansible commands

---

🤔 **WHY is this required?**

* The DevOps team wants to automate configuration and server management
* Ansible is **agentless**, simple, and widely used
* Jump host is the central access point to all servers
* Installing Ansible here allows:

  * Managing app servers
  * Running automation playbooks
  * Testing configuration changes safely

Without Ansible:
❌ Manual server management
❌ No scalable automation

---

⏰ **WHEN is this used in real life?**

* Server configuration
* Application deployment
* Patch management
* User & permission management
* CI/CD pipelines
* Cloud & on-prem automation

Basically → **daily DevOps work**

---

🧠 **HOW Ansible Works (Beginner Explanation)**

* Ansible uses **SSH** to connect to servers
* No agent needs to be installed on managed nodes
* One machine acts as **controller** (jump host)
* Ansible runs tasks written in **YAML playbooks**

Flow:
Controller (jump host) ➝ SSH ➝ Managed servers

---

🖥️ **WHERE will Ansible be installed?**

* Server: **jump_host**
* User used for install: **thor**
* Tool used: **pip3**
* Scope: **global (all users)**

---

⚙️ **HOW to Solve the Task (Step-by-Step)**

---

🔹 **Step 1: Login to Jump Host**

WHY:
Ansible must be installed on the controller node.

```bash
ssh thor@jump_host
```

---

🔹 **Step 2: Check pip3 Availability**

WHY:
Task explicitly says to use **pip3 only**.

```bash
pip3 --version
```

If pip3 exists → proceed ✅

---

🔹 **Step 3: Install Ansible 4.7.0 Using pip3**

WHY pip3?

* Task requirement
* Allows precise version control

WHY sudo?

* To install Ansible **globally**
* So all users can access it

```bash
sudo pip3 install ansible==4.7.0
```

HOW this works:

* Installs Ansible binaries in `/usr/local/bin`
* Makes `ansible` command available system-wide

---

🔹 **Step 4: Verify Ansible Installation**

WHY:
Always verify tool installation.

```bash
ansible --version
```

Expected output should show:

* Ansible version: **4.7.0**

---

🔹 **Step 5: Confirm Global Availability**

WHY:
The task says **all users must be able to run Ansible**.

```bash
which ansible
```

Expected path:

```
/usr/local/bin/ansible
```

This path is accessible to all users → ✅

---

🎉 **FINAL RESULT (WHAT Changed?)**

✔ Ansible 4.7.0 installed successfully
✔ Installed using pip3 as required
✔ Ansible available globally
✔ Jump host is ready as Ansible controller

---

🧠 **BEGINNER KEY TAKEAWAYS**

* Ansible controller manages other servers
* pip3 allows version-specific installation
* Global install ensures team-wide usability
* `ansible --version` is your first verification step

---

⚠️ **COMMON BEGINNER MISTAKES**

❌ Installing Ansible using yum/apt (not allowed here)
❌ Installing without sudo (user-only install)
❌ Installing wrong Ansible version
❌ Forgetting to verify installation

---


