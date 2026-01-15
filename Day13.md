# 🟢 Day 13: Secure Apache Port 5000 Using iptables 🔐🛡️

---

## 📌 **Task Details (As Given)**

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 5000 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:

1. Install iptables and all its dependencies on each app host.
2. Block incoming port 5000 on all apps for everyone except for LBR host.
3. Make sure the rules remain, even after system reboot.

---

## 🤔 **WHAT was the problem?**

* Apache was running on **port 5000**
* No firewall was present
* Anyone could directly access app servers
* This violated security best practices

---

## 🤔 **WHY is this a security risk?**

* Open ports are visible to attackers
* Direct access bypasses load balancer protections
* In production:

  * App servers should **never be publicly accessible**
  * Only Load Balancer should talk to app servers

---

## 🧠 **TARGET ARCHITECTURE (Correct Design)**

```
User
  ↓
Load Balancer (stlb01)
  ↓
App Servers (Apache :5000)
```

✔ Users → LBR
❌ Users → App Servers (blocked)

---

## 🧠 **WHAT is iptables? (Beginner Explanation)**

* iptables is a **Linux firewall**
* Controls network traffic at OS level
* Uses rule-based filtering
* Processes rules **top to bottom**

---

## 🧠 **Important iptables Concepts**

* INPUT chain → incoming traffic
* ACCEPT → allow traffic
* REJECT → block traffic
* Rule order matters (first match wins)

---

## 🖥️ **Infrastructure Context**

* Load Balancer IP: `172.16.238.14`
* App Servers:

  * `stapp01`
  * `stapp02`
  * `stapp03`
* Apache Port: `5000`

---

## ⚙️ **Step-by-Step: How We Solved the Task**

---

### 🔹 Step 1: Login to App Server

```bash
ssh tony@stapp01
```

---

### 🔹 Step 2: Install iptables & dependencies

```bash
sudo yum install -y iptables iptables-services
```

---

### 🔹 Step 3: Start & Enable iptables

```bash
sudo systemctl start iptables
sudo systemctl enable iptables
```

Verify:

```bash
sudo systemctl status iptables
```

---

### 🔹 Step 4: Allow Apache Port 5000 from LBR Only

```bash
sudo iptables -I INPUT -p tcp -s 172.16.238.14 --dport 5000 -j ACCEPT
```

✔ Allows traffic **only from LBR**

---

### 🔹 Step 5: Block Port 5000 for Everyone Else

```bash
sudo iptables -A INPUT -p tcp --dport 5000 -j REJECT
```

✔ Blocks all other sources

---

### 🔹 Step 6: Verify Rule Order

```bash
sudo iptables -L -n --line-numbers
```

Correct order:
1️⃣ ACCEPT rule for LBR
2️⃣ REJECT rule for port 5000

---

### 🔹 Step 7: Save iptables Rules (Persistence)

```bash
sudo iptables-save | sudo tee /etc/sysconfig/iptables
```

✔ Rules will survive reboot

---

### 🔹 Step 8: Test Connectivity

**From Load Balancer:**

```bash
curl http://stapp01:5000
```

✅ Should work

**From Jump Host:**

```bash
curl http://stapp01:5000
```

❌ Should fail

---

### 🔹 Step 9: Repeat on All App Servers

Repeat **Steps 1–8** on:

* `stapp02`
* `stapp03`

---

## 🎉 **FINAL RESULT**

✔ iptables installed on all app servers
✔ Port 5000 secured
✔ Only LBR can access app servers
✔ Firewall rules persist after reboot
✔ Security posture improved

---

## 🧠 **BEGINNER KEY LEARNINGS**

* Never expose app servers directly
* Firewalls enforce least privilege
* iptables rules are order-sensitive
* Always save rules for persistence
* Load balancer is the only gateway

---

## ⚠️ **COMMON MISTAKES TO AVOID**

❌ Blocking before allowing LBR
❌ Forgetting to save iptables rules
❌ Applying rules on wrong server
❌ Disabling firewall instead of configuring it

---

## 📝 **INTERVIEW-READY EXPLANATION**

> We secured Apache by installing iptables, allowing port 5000 access only from the load balancer, blocking all other sources, and saving the rules to persist across reboots.

---

