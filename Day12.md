# 🟢 Day 12: Apache Not Reachable on Port 3001 — How We Solved It 🌐🛠️

---

## 📌 **Task Summary**

Apache service on **App Server 1 (stapp01)** was reported as **not reachable on port 3001** from the **jump host**.
The issue could be due to:

* Apache service down
* Port conflict
* Firewall / iptables rules
* Network binding issues

Final validation required:

```bash
curl http://stapp01:3001
```

---

## 🤔 **WHAT was the problem?**

* Apache worked **locally** on `stapp01`
* Apache **failed** when accessed from **jump host**
* Error from jump host:

  ```
  No route to host
  ```

This indicated a **network-level block**, not an Apache application issue.

---

## 🧠 **WHY this happened**

* Port **3001** was initially occupied by **sendmail**
* Apache could not start due to port conflict
* After freeing the port and starting Apache:

  * Apache listened correctly on port 3001
* However, **iptables firewall rules** blocked external access

---

## ⏰ **WHEN such issues occur in real life**

* Custom ports used for services
* Firewall rules allow only SSH by default
* Multiple services compete for the same port
* Security-hardened servers with REJECT rules

---

## 🧠 **HOW we approached the problem (Professional Flow)**

Instead of guessing, we followed a strict troubleshooting order:

1️⃣ Check Apache service status
2️⃣ Validate Apache configuration
3️⃣ Check which process is using the port
4️⃣ Verify Apache listening address
5️⃣ Test locally vs remotely
6️⃣ Inspect firewall / iptables rules

---

## ⚙️ **Step-by-Step: How We Fixed the Issue**

---

### 🔹 Step 1: Checked Apache Service Status

```bash
sudo systemctl status httpd
```

Apache failed while reading configuration, so we investigated further.

---

### 🔹 Step 2: Verified Apache Configuration

```bash
sudo apachectl configtest
```

Output:

```
Syntax OK
```

✔ Confirmed configuration was valid.

---

### 🔹 Step 3: Identified Port Conflict

```bash
sudo netstat -tulnp | grep 3001
```

Output:

```
127.0.0.1:3001 LISTEN sendmail
```

🔴 Root cause:

* **sendmail** was already using port 3001
* Apache could not bind to the required port

---

### 🔹 Step 4: Freed Port 3001

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

Verified port was free:

```bash
sudo netstat -tulnp | grep 3001
```

---

### 🔹 Step 5: Started Apache Successfully

```bash
sudo systemctl start httpd
sudo systemctl status httpd
```

Apache was now running correctly.

---

### 🔹 Step 6: Confirmed Apache Listening on Port 3001

```bash
sudo netstat -tulnp | grep httpd
```

Expected:

```
0.0.0.0:3001 LISTEN httpd
```

---

### 🔹 Step 7: Tested Locally (Worked)

```bash
curl http://localhost:3001
```

✔ Apache responded successfully.

---

### 🔹 Step 8: Identified Network-Level Block

From **jump host**:

```bash
curl http://stapp01:3001
```

Error:

```
No route to host
```

This confirmed:

* Apache is fine
* Network firewall is blocking traffic

---

### 🔹 Step 9: Checked Firewall Type

* `firewalld` was **not installed**
* Server was using **iptables**

Checked rules:

```bash
sudo iptables -L -n
```

Found:

```text
REJECT all -- 0.0.0.0/0 reject-with icmp-host-prohibited
```

Port 3001 was **not allowed**.

---

### 🔹 Step 10: Allowed Port 3001 in iptables

Inserted rule **before REJECT**:

```bash
sudo iptables -I INPUT 4 -p tcp --dport 3001 -j ACCEPT
```

Verified rule order:

```bash
sudo iptables -L -n --line-numbers
```

Saved rules:

```bash
sudo iptables-save | sudo tee /etc/sysconfig/iptables
```

---

### 🔹 Step 11: Final Validation from Jump Host

```bash
curl http://stapp01:3001
```

🎉 Apache became reachable successfully.

---

## 🎉 **Final Outcome**

✔ Apache running on port 3001
✔ Port conflict resolved
✔ Firewall rule correctly updated
✔ Network access restored
✔ Security preserved
✔ Monitoring issue fixed

---

## 🧠 **BEGINNER KEY LEARNINGS**

* Local access ≠ network access
* Port conflicts can silently break services
* iptables rules are order-dependent
* REJECT rules override everything below
* Always test from the client side
* Never disable firewall blindly

---

## ⚠️ **COMMON MISTAKES TO AVOID**

❌ Assuming localhost success means service is accessible
❌ Ignoring firewall when service works locally
❌ Killing services without checking port usage
❌ Disabling firewall instead of adding rules

---

## 📝 **INTERVIEW-READY EXPLANATION**

> Apache was unreachable due to a port conflict and restrictive iptables rules. After freeing the port and explicitly allowing TCP traffic on port 3001 before the REJECT rule, Apache became accessible from the jump host.

---

