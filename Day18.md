# 🌐 Day 18: WordPress Application Setup (Apache + PHP + MariaDB)

---

## 📌 Task Details (As Provided)

xFusionCorp Industries is planning to host a **WordPress website** on their infrastructure in **Stratos Datacenter**. The infrastructure configuration is already done.

On the **storage server**, a shared directory `/vaw/www/html` is mounted on each **App Server** under:

```
/var/www/html
```

### ✅ Requirements

a. Install **httpd**, **php**, and its dependencies on **all App Servers**.
b. Configure Apache to listen on **port 8084** on App Servers.
c. Install and configure **MariaDB server** on the **DB Server**.
d. Create:

* Database: `kodekloud_db5`
* User: `kodekloud_joy`
* Password: `BruCStnMT5`
* Grant full permissions to the user on this database.
  e. Final verification:
* Access the website via **LBR App button**
* Message should appear:

  ```
  App is able to connect to the database using user kodekloud_joy
  ```

---

## 🧠 Discussion: Why, When & How (Beginner Guide)

---

### ❓ Why are we doing this?

WordPress is a **PHP-based application** that requires:

* A **web server** (Apache)
* A **runtime** (PHP)
* A **database** (MariaDB)

To handle traffic efficiently and avoid downtime, the application is deployed using:

* Multiple App Servers
* A centralized Database Server
* A Load Balancer (LBR)
* Shared storage for website files

This setup provides:

* High Availability
* Scalability
* Better performance

---

### ⏰ When do we use this setup?

This architecture is used:

* In production environments
* When traffic is high
* When applications must stay online 24×7
* For popular CMS platforms like WordPress

---

### ⚙️ How does this setup work?

```
User
 ↓
Load Balancer (LBR)
 ↓
App Servers (Apache + PHP)
 ↓
MariaDB Database Server
```

* Apache serves the website
* PHP executes WordPress logic
* MariaDB stores application data
* Shared storage ensures same website files across all App Servers

---

## 🪜 Step-by-Step Solution

---

## 🔹 STEP 1: Install Apache & PHP on All App Servers

> Perform these steps on **stapp01, stapp02, stapp03**

### 📥 Install required packages

```bash
yum install -y httpd php php-mysqlnd
```

🧠 `php-mysqlnd` allows PHP to communicate with MariaDB.

---

### ▶️ Start & enable Apache

```bash
systemctl start httpd
systemctl enable httpd
```

---

## 🔹 STEP 2: Configure Apache to Use Port 8084

### ✏️ Edit Apache configuration

```bash
vi /etc/httpd/conf/httpd.conf
```

Find:

```text
Listen 80
```

Change to:

```text
Listen 8084
```

Ensure DocumentRoot is:

```text
DocumentRoot "/var/www/html"
```

Save and exit.

---

### 🔄 Restart Apache

```bash
systemctl restart httpd
```

### ✅ Verify port

```bash
ss -tulnp | grep httpd
```

Expected:

```
0.0.0.0:8084
```

---

## 🔹 STEP 3: Install and Configure MariaDB on DB Server

### 🔐 Login to DB Server

```bash
ssh peter@stdb01
```

---

### 📥 Install MariaDB

```bash
yum install -y mariadb-server
```

---

### ▶️ Start & enable MariaDB

```bash
systemctl start mariadb
systemctl enable mariadb
```

---

## 🔹 STEP 4: Create Database and User

### 🔑 Login to MariaDB

```bash
mysql -u root
```

---

### 🗄️ Create database

```sql
CREATE DATABASE kodekloud_db5;
```

---

### 👤 Create database user

```sql
CREATE USER 'kodekloud_joy'@'%' IDENTIFIED BY 'BruCStnMT5';
```

---

### 🔐 Grant full permissions

```sql
GRANT ALL PRIVILEGES ON kodekloud_db5.* TO 'kodekloud_joy'@'%';
FLUSH PRIVILEGES;
```

---

### 🚪 Exit MariaDB

```sql
EXIT;
```

---

## 🔹 STEP 5: Final Verification

### 🌍 Click **App** button from the top bar (LBR)

✅ Expected Output:

```
App is able to connect to the database using user kodekloud_joy
```

This confirms:

* Apache is running
* PHP is working
* Database is reachable
* Credentials are correct
* Network flow is healthy

---

## ⚠️ Common Beginner Mistakes (Avoid These)

❌ Forgetting to change Apache port<br>
❌ Not installing `php-mysqlnd`<br>
❌ Database user without permissions<br>
❌ Apache not restarted after config change<br>
❌ Mixing App Server & DB Server steps

---

## 🎯 Key Learnings

* WordPress requires Apache + PHP + DB
* Shared storage is essential for multi-server apps
* Load Balancer handles traffic distribution
* Database setup is critical for app connectivity
* Always verify end-to-end using LBR/App button

---

## 🧠 Interview-Ready Explanation

> I configured a WordPress-style application by setting up Apache and PHP on multiple App Servers, configuring Apache to listen on a custom port, provisioning a MariaDB database with proper user privileges, and validating end-to-end connectivity through the Load Balancer.

---
