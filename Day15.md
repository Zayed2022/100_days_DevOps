# 🟢 Day 14: Nginx + SSL Configuration on App Server 3 🔐🌐

---

## 🎯 Task Goal (In Simple Words)

Prepare **App Server 3** for application deployment by:

* Installing **Nginx**
* Enabling **HTTPS (SSL)**
* Using a **self-signed certificate**
* Serving a basic webpage
* Verifying access from **Jump Host**

This ensures the server is **secure, ready, and validated** before real app deployment.

---

## 🧠 WHAT did we configure?

* Web Server: **Nginx**
* Protocol: **HTTPS**
* Port: **443**
* Certificate Type: **Self-signed**
* Web content: `Welcome!`

---

## 🤔 WHY are we doing this?

### 🔐 Security

* HTTPS encrypts traffic
* Prevents data leakage
* Required for modern applications

### 🧱 Proper Server Preparation

* Applications should never be deployed on:

  * Unsecured servers
  * Unvalidated web stacks

### 🧪 Pre-Deployment Validation

* Simple `index.html` confirms:

  * Web server works
  * SSL works
  * Network access works

---

## ⏰ WHEN is this done in real life?

* Before deploying:

  * Java apps
  * Python apps
  * React / frontend apps
* During:

  * Server provisioning
  * Blue-green deployments
  * Infrastructure readiness phase

This is **Day-0 / Day-1 DevOps work**.

---

## 🧠 BEGINNER CONCEPTS YOU MUST UNDERSTAND

---

### 🌐 What is Nginx?

* A **web server**
* Serves HTML pages
* Handles HTTPS traffic
* Very fast & lightweight
* Commonly used in production

---

### 🔐 What is SSL / HTTPS?

* SSL encrypts communication
* HTTPS = HTTP + SSL
* Uses:

  * `.crt` → certificate
  * `.key` → private key

---

### ⚠️ What is a Self-Signed Certificate?

* Generated internally
* Not trusted by browsers
* Perfect for:

  * Internal apps
  * Testing
  * Dev / staging

That’s why we use:

```bash
curl -k
```

---

### 🚫 Why `/tmp` is NOT safe for SSL files?

* Temporary directory
* Files can disappear
* Weak permissions
* Not suitable for production

👉 SSL files must be stored under `/etc`

---

## 🛠️ HOW we solved it (Step-by-Step Learning)

---

### 🔹 Step 1: Login to App Server 3

```bash
ssh banner@stapp03
```

👉 Always configure services **on the target server**.

---

### 🔹 Step 2: Install Nginx

```bash
sudo yum install -y nginx
```

Start & enable it:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Check status:

```bash
sudo systemctl status nginx
```

---

### 🔹 Step 3: Secure SSL Certificates

Create secure directory:

```bash
sudo mkdir -p /etc/nginx/ssl
```

Move cert & key:

```bash
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```

Set permissions (VERY IMPORTANT):

```bash
sudo chmod 600 /etc/nginx/ssl/nautilus.key
sudo chmod 644 /etc/nginx/ssl/nautilus.crt
```

🔐 Private key must be readable only by root.

---

### 🔹 Step 4: Configure HTTPS in Nginx

SSL configuration tells Nginx:

* Which port to listen on
* Which certificate to use
* Where website files are

Example server block:

```nginx
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    root /usr/share/nginx/html;
    index index.html;
}
```

---

### 🔹 Step 5: Create Website Content

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

👉 This confirms:

* Nginx serves files
* Document root is correct

---

### 🔹 Step 6: Validate Nginx Configuration

```bash
sudo nginx -t
```

Expected:

```
syntax is ok
test is successful
```

---

### 🔹 Step 7: Restart Nginx

```bash
sudo systemctl restart nginx
```

---

### 🔹 Step 8: Local HTTPS Test

```bash
curl -Ik https://localhost
```

Confirms:

* SSL handshake
* Nginx response
* Port 443 active

---

### 🔹 Step 9: Final Test from Jump Host 🌍

```bash
curl -Ik https://stapp03
```

or

```bash
curl -Ik https://<app-server-3-ip>
```

`-I` → headers only
`-k` → ignore self-signed cert warning

✅ HTTP 200 OK = SUCCESS

---

## 🎉 FINAL RESULT

✔ Nginx installed
✔ SSL configured
✔ Certificates secured
✔ HTTPS enabled
✔ Web content served
✔ Access verified from jump host

---

## 🧠 KEY LEARNINGS (VERY IMPORTANT)

* Always move certs out of `/tmp`
* SSL needs both `.crt` and `.key`
* `nginx -t` saves you from outages
* HTTPS testing needs `-k` for self-signed certs
* Always test from **client side**

---

## ❌ COMMON BEGINNER MISTAKES

* Leaving certs in `/tmp`
* Forgetting to restart nginx
* Wrong file permissions on `.key`
* Testing only locally
* Skipping `nginx -t`

---

## 🧠 INTERVIEW-READY EXPLANATION

> I prepared App Server 3 by installing Nginx, configuring HTTPS using a self-signed certificate, securing SSL files in `/etc/nginx/ssl`, creating a validation webpage, and verifying encrypted access from the jump host.

---


Whenever you’re ready, send the **next task** 💪
We’ll again follow the same powerful learning flow 🔥
