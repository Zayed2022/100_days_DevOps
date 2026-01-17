# 🐘 Day 17: Install and Configure PostgreSQL

---

## 📌 Task Details (As Provided)

The Nautilus application development team has shared that they are planning to deploy one newly developed application on **Nautilus infrastructure** in **Stratos Datacenter**. The application uses **PostgreSQL** database, so as a **pre-requisite** we need to set up PostgreSQL database server as per requirements shared below:

* PostgreSQL database server is already installed on the Nautilus database server.

### ✅ Requirements

a. Create a database user **kodekloud_cap** and set its password to **YchZHRcLkL**.<br>
b. Create a database **kodekloud_db9** and grant **full permissions** to user **kodekloud_cap** on this database.

📌 **Note:**
Do **NOT** restart the PostgreSQL service.

---

## 🧠 Discussion: Why, When & How (Beginner Guide)

---

### ❓ Why are we doing this?

Applications **do not connect directly** to the operating system.
They connect to a **database** using:

* A **database user**
* A **password**
* A **database name**

To allow the new application to work, we must:

* Create a dedicated database
* Create a dedicated user
* Give that user permission to use the database

This is a **standard production practice**.

---

### ⏰ When do we do this?

This is done:

* **Before deploying an application**
* During **pre-requisite setup**
* While preparing backend services (DB, cache, queues)

This is often called **Day-0 / Day-1 setup** in DevOps.

---

### ⚙️ How does this work?

PostgreSQL:

* Has its **own users** (different from Linux users)
* Uses **SQL commands** for configuration
* Does NOT require a restart for user/database creation

We:

1. Log in as the PostgreSQL admin user
2. Use the `psql` shell
3. Run SQL commands to create user and database
4. Grant permissions

---

## 🪜 Step-by-Step Solution

---

### 🔹 Step 1: Login to Database Server

```bash
ssh peter@stdb01
```

📌 This is the **Nautilus database server**.

---

### 🔹 Step 2: Switch to PostgreSQL Admin User

```bash
sudo su - postgres
```

🧠 The `postgres` user:

* Is the default PostgreSQL superuser
* Has permission to create users and databases

---

### 🔹 Step 3: Enter PostgreSQL Shell

```bash
psql
```

Expected prompt:

```
postgres=#
```

✔ This confirms you are inside PostgreSQL.

---

### 🔹 Step 4: Create Database User

```sql
CREATE USER kodekloud_cap WITH PASSWORD 'YchZHRcLkL';
```

🧠 This creates:

* A PostgreSQL login user
* With the specified password

---

### 🔹 Step 5: Create Database

```sql
CREATE DATABASE kodekloud_db9;
```

🧠 This creates an empty database for the application.

---

### 🔹 Step 6: Grant Full Permissions

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db9 TO kodekloud_cap;
```

🧠 This allows the user to:

* Read data
* Insert / update / delete data
* Create tables and objects

---

### 🔹 Step 7: Exit PostgreSQL Shell

```sql
\q
```

---

### 🔹 Step 8: Exit postgres User

```bash
exit
```

---

## ✅ Final Result

✔ PostgreSQL user created
✔ Database created
✔ Permissions granted
✔ PostgreSQL service untouched (no restart)

---

## ⚠️ Common Beginner Mistakes (Avoid These)

❌ Restarting PostgreSQL unnecessarily <br>
❌ Running SQL commands outside `psql` <br>
❌ Forgetting quotes around passwords  <br>
❌ Granting permissions before database creation

---

## 🎯 Key Learnings

* PostgreSQL users are **different from Linux users**
* Databases must be created before granting access
* Many DB changes are **dynamic** (no restart needed)
* Always follow task constraints carefully

---

## 🧠 Interview-Ready Explanation

> I configured PostgreSQL by creating a dedicated database user and database, then granted full privileges to the user without restarting the service, ensuring the environment was ready for application deployment.

---

