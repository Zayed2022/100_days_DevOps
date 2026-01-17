# 📌 Day: Nginx Load Balancer Setup (High Availability)


## 1️⃣ Task Details (As Provided)

Day-by-day traffic is increasing on one of the websites managed by the **Nautilus** production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a **high availability stack** i.e. on **Nautilus infrastructure** in **Stratos Datacenter**.

The migration is almost complete, and only the **LBR (Load Balancer)** server configuration is pending.

### ✅ Requirements

* Install **Nginx** on the **LBR (Load Balancer)** server.
* Configure **load balancing** inside the `http` context using **all App Servers**.
* Update **only the main Nginx configuration file**:

  ```
  /etc/nginx/nginx.conf
  ```
* **Do NOT change the Apache port** already configured on App Servers.
* Ensure **Apache (httpd)** is running on all App Servers.
* **Verification**: Access the website using the **StaticApp** button from the top bar.

---

## 2️⃣ Discussion: Why, When & How (Beginner Friendly)

---

### ❓ Why are we doing this?

When traffic increases and only **one server** handles all requests:

* The server becomes slow 🐌
* Website response time increases
* Single point of failure ❌

A **Load Balancer** solves this problem.

🔀 It distributes traffic across **multiple App Servers**, so:

* No single server is overloaded
* Performance improves
* Website stays available even if one server fails

👉 This concept is called **High Availability (HA)**.

---

### ⏰ When do we use a Load Balancer?

We use a load balancer when:

* Traffic is high
* Application is business-critical
* Downtime is not acceptable
* Multiple backend servers exist

👉 This is **very common in real production systems**.

---

### ⚙️ How does Nginx Load Balancing work?

Nginx acts as a **reverse proxy**.

Basic flow:

```
User / Browser
     ↓
Load Balancer (Nginx on LBR)
     ↓
App Server 1 (Apache)
App Server 2 (Apache)
App Server 3 (Apache)
```

Key concepts used:

* **upstream** → defines backend servers
* **proxy_pass** → forwards requests to backend group
* **http context** → where load balancing logic lives

📌 In this task:

* Apache is already running on **port 5002**
* Nginx will forward traffic to:

  ```
  stapp01:5002
  stapp02:5002
  stapp03:5002
  ```

---

## 3️⃣ Step-by-Step Solution

---

### 🪜 Step 1: Access the LBR Server

Login to the **Load Balancer server (stlb01)**.

```bash
ssh loki@stlb01
sudo su -
```

📌 We configure load balancing **only on the LBR server**, not on App Servers.

---

### 🪜 Step 2: Install and Start Nginx

Install Nginx and ensure it runs automatically.

```bash
yum install -y nginx
systemctl start nginx
systemctl enable nginx
```

Verify:

```bash
systemctl status nginx
```

✔ Should show: `active (running)`

---

### 🪜 Step 3: Identify Apache Port on App Servers

We must **not guess** the Apache port.

Check on any App Server:

```bash
sudo ss -tunlp | grep httpd
```

📌 In this environment, Apache is running on:

```
port 5002
```

⚠️ This port must be used in the Nginx configuration.

---

### 🪜 Step 4: Configure Nginx Load Balancer

Open the **main Nginx configuration file** (important!).

```bash
vi /etc/nginx/nginx.conf
```

---

#### 🔹 Add the `upstream` block (inside `http {}`)

```nginx
http {

    upstream app_servers {
        server stapp01:5002;
        server stapp02:5002;
        server stapp03:5002;
    }

    ...
}
```

🧠 This tells Nginx:

> “These are my backend App Servers.”

---

#### 🔹 Update the `server` block (inside `http {}`)

Find the default server block and update `location /`:

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://app_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

🧠 What this does:

* Nginx listens on port **80**
* Forwards requests to the backend servers
* Preserves client IP information

---

### 🪜 Step 5: Test and Restart Nginx

Always validate configuration before restart.

```bash
nginx -t
```

Expected output:

```
syntax is ok
test is successful
```

Restart Nginx:

```bash
systemctl restart nginx
```

---

### 🪜 Step 6: Final Verification

Click the **StaticApp** button from the KodeKloud interface.

✅ Expected Result:

* Website loads successfully
* No `502 Bad Gateway`
* Load balancing works correctly

---

## 🎯 Key Learnings (Beginner Takeaways)

* Load balancer improves **performance & availability**
* `upstream` defines backend servers
* `proxy_pass` forwards traffic
* Always verify **backend service + port**
* Never edit random config files → follow task constraints
* `nginx -t` is mandatory before restart

---

## 🧠 Interview-Ready Explanation

> I configured Nginx as a load balancer on the LBR server by defining upstream App Servers in the http context and forwarding traffic using proxy_pass, ensuring Apache services were reused without port changes and validating the setup using the StaticApp endpoint.

---


Just say **next** 💪
