# 🐘 Day XX: NGINX + PHP-FPM 8.3 Configuration (Unix Socket)

---

## 📌 Task Summary

The Nautilus application development team deployed a **PHP-based application** that requires **NGINX** and **PHP-FPM** to work together.
The goal was to configure NGINX to serve PHP files using **PHP-FPM 8.3** via a **Unix socket**, without modifying application files.

---

## ✅ Final Objectives (What We Achieved)

✔️ Installed and configured **NGINX**
✔️ Configured NGINX to:

* Listen on **port 8095**
* Use **/var/www/html** as document root

✔️ Installed **PHP-FPM version 8.3 (only)**
✔️ Configured PHP-FPM to use Unix socket:

```
/var/run/php-fpm/default.sock
```

✔️ Integrated **NGINX + PHP-FPM**
✔️ Validated setup using `curl` from **Jump Host**

---

## 🧠 Why This Setup Is Needed (Beginner Explanation)

### ❓ Why NGINX + PHP-FPM?

* **NGINX cannot execute PHP code directly**
* PHP execution is handled by **PHP-FPM**
* NGINX forwards `.php` requests to PHP-FPM
* PHP-FPM processes the script and sends output back to NGINX

This separation is:

* Faster
* More secure
* Industry standard for production

---

### 🔌 Why use a Unix socket instead of TCP?

Using a Unix socket:

* Is **faster** (no network stack)
* Is **local only** (more secure)
* Is widely used in production environments

That’s why the task explicitly required:

```
/var/run/php-fpm/default.sock
```

---

## 🧠 Architecture Flow (Very Important)

```
Client / Jump Host
        ↓
NGINX (port 8095)
        ↓
PHP-FPM 8.3 (Unix Socket)
        ↓
PHP Script Execution
```

---

## 🪜 Step-by-Step Implementation

---

## 🔹 Step 1: Install & Configure NGINX

### Install NGINX

```bash
sudo yum install -y nginx
```

### Configure NGINX

Edit main config:

```bash
sudo vi /etc/nginx/nginx.conf
```

### Required server block configuration:

```nginx
server {
    listen 8095;
    server_name _;
    root /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        include fastcgi.conf;
    }
}
```

🧠 This ensures:

* PHP files are sent to PHP-FPM
* Static files are served directly by NGINX

---

## 🔹 Step 2: Install PHP-FPM 8.3 (Important)

### Enable PHP 8.3 module

```bash
sudo yum module reset php -y
sudo yum module enable php:8.3 -y
```

### Install PHP-FPM

```bash
sudo yum install -y php php-fpm php-cli php-common
```

### Verify version

```bash
php -v
php-fpm -v
```

✅ Output must show **PHP 8.3**

---

## 🔹 Step 3: Configure PHP-FPM Socket

Edit pool configuration:

```bash
sudo vi /etc/php-fpm.d/www.conf
```

### Change only the required line:

```ini
listen = /var/run/php-fpm/default.sock
```

(Optional but safe if present)

```ini
listen.acl_users = apache,nginx
```

🧠 This allows **NGINX** to communicate with PHP-FPM via socket.

---

## 🔹 Step 4: Start & Enable Services

```bash
sudo systemctl start php-fpm
sudo systemctl enable php-fpm

sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 🔹 Step 5: Validate Configuration

### Check NGINX syntax

```bash
sudo nginx -t
```

### Verify socket exists

```bash
ls -l /var/run/php-fpm/default.sock
```

---

## 🔹 Step 6: Final Verification (From Jump Host)

```bash
curl http://stapp01:8095/index.php
curl http://stapp01:8095/info.php
```

✅ PHP output confirms:

* NGINX is running
* PHP-FPM 8.3 is active
* Socket integration works

---

## ⚠️ Common Beginner Mistakes (Avoid These)

❌ Installing default PHP instead of **8.3**
❌ Forgetting to enable PHP module stream
❌ Socket path mismatch between NGINX and PHP-FPM
❌ Restarting NGINX before PHP-FPM
❌ Editing application files (`index.php`, `info.php`)

---

## 🎯 Key Learnings

* NGINX + PHP-FPM is a **standard production setup**
* PHP version matters (validators are strict)
* Unix sockets are preferred over TCP
* Minimal config changes reduce errors
* Always verify versions before restarting services

---

## 🧠 Interview-Ready Explanation

> I configured NGINX to serve PHP applications by integrating it with PHP-FPM 8.3 over a Unix socket, ensuring correct port configuration, document root usage, and proper FastCGI handling, and validated the setup using curl from the jump host.

---


