🟢 **Task**
Day 7: Linux SSH Authentication 🔐

---

📌 **Task Details**

The system admins team of **xFusionCorp Industries** has set up automation scripts on the **jump host**.
These scripts run at regular intervals and perform operations on all **app servers** in the **Stratos Datacenter**.

For these scripts to work properly, the user **thor** on the jump host must have **password-less SSH access** to all app servers using their respective sudo users (for example, **tony**).

---

🤔 **What is Password-less SSH?**

Normally, SSH asks for a password every time you connect to a server.
Password-less SSH allows secure access **without entering a password**, which is essential for automation.

---

🔑 **How SSH Authentication Works (Simple)**

SSH uses two keys:

🔐 **Private Key**

* Stored on the jump host
* Never shared

🔓 **Public Key**

* Copied to target servers
* Stored in `authorized_keys`

If both keys match → access granted automatically ✅

---

🖥️ **Servers & Users Involved**

Jump Host
👤 User: thor

App Servers
🖥️ stapp01 → user: tony
🖥️ stapp02 → user: tony
🖥️ stapp03 → user: tony

---

⚙️ **Solution Steps (Beginner-Friendly)**

---

🔹 **Step 1: Login to Jump Host**

```bash
ssh thor@jump_host
```

---

🔹 **Step 2: Generate SSH Key Pair**

```bash
ssh-keygen -t rsa
```

When prompted:

* Press **Enter** to accept default path
* Press **Enter** twice to keep passphrase empty

Default key location:

```
/home/thor/.ssh/id_rsa
/home/thor/.ssh/id_rsa.pub
```

📝 No passphrase is used because automation scripts cannot provide one.

---

🔹 **Step 3: Copy Public Key to App Server 1**

```bash
ssh-copy-id tony@stapp01
```

You will enter the password **only once**.

---

🔹 **Step 4: Copy Public Key to App Server 2**

```bash
ssh-copy-id tony@stapp02
```

---

🔹 **Step 5: Copy Public Key to App Server 3**

```bash
ssh-copy-id tony@stapp03
```

---

🔹 **Step 6: Verify Password-less SSH**

```bash
ssh tony@stapp01
ssh tony@stapp02
ssh tony@stapp03
```

✅ You should not be asked for any password.

---

🎉 **Final Outcome**

✔ Password-less SSH configured successfully
✔ Jump host can access all app servers
✔ Automation scripts can run without interruption

---

🧠 **Key Learnings**

* SSH keys are more secure than passwords
* Password-less SSH is mandatory for automation
* Private key stays on source machine
* Public key is copied to target servers

---

⚠️ **Common Mistakes to Avoid**

❌ Generating SSH keys on app servers
❌ Using root instead of sudo users
❌ Setting a passphrase for automation keys
❌ Copying private key to servers

---

