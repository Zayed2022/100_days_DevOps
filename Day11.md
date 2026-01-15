**Day 11: Java Application Deployment & Tomcat Troubleshooting ☕🚀**

---

📌 **Task Details (As Given)**

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server.

Based on the requirements mentioned below complete the task:

a. Install tomcat server on App Server 1.

b. Configure it to run on port 3000.

c. There is a ROOT.war file on Jump host at location /tmp.

Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e
`curl http://stapp01:3000`

---


## 🔹 Step 1: Installed Tomcat on App Server 1

**Why this step?**  
Tomcat is required to run Java web applications (.war files).

**What we did:**
```bash
ssh tony@stapp01
sudo yum install -y tomcat
```

This installed the Tomcat application server.

---

## 🔹 Step 2: Changed Tomcat Port to 3000

**Why this step?**  
By default, Tomcat runs on port 8080, but the task required it to run on 3000.

**What we did:**
```bash
sudo vi /etc/tomcat/server.xml
```

Modified the Connector port from:
```xml
port="8080"
```
to:
```xml
port="3000"
```

---

## 🔹 Step 3: Started and Enabled Tomcat

**Why this step?**  
Tomcat must be running for the application to be accessible.

**What we did:**
```bash
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

---

## 🔹 Step 4: Deployed ROOT.war File

**Why this step?**  
Java applications are deployed in Tomcat using `.war` files.  
`ROOT.war` ensures the app opens at the base URL.

**What we did:**

From Jump Host:
```bash
scp /tmp/ROOT.war tony@stapp01:/tmp
```

On App Server 1:
```bash
sudo mv /tmp/ROOT.war /usr/share/tomcat/webapps/
sudo systemctl restart tomcat
```

---

## 🔹 Step 5: Faced the Issue (Connection Refused)

**What happened?**
```bash
curl http://stapp01:3000
```

**Result:**
```
Connection refused
```

This showed that:
- Tomcat was running  
- But it was not accessible on port 3000  

---

## 🔹 Step 6: Checked Which Port Tomcat Was Listening On

**Why this step?**  
To verify whether Tomcat was actually listening on port 3000.

**What we did:**
```bash
sudo netstat -tulnp | grep java
```

**Output:**
```
127.0.0.1:3000   LISTEN
0.0.0.0:8080     LISTEN
```

---

## 🔹 Step 7: Identified the Root Cause

**What we learned:**
- Port 3000 was bound to `127.0.0.1` (localhost only)  
- Port 8080 was bound to `0.0.0.0` (all interfaces)  

👉 This meant:
- External requests to `stapp01:3000` were rejected  
- Tomcat was not accessible over the network on port 3000  

---

## 🔹 Step 8: Fixed the Tomcat Connector Binding

**Why this step?**  
To allow Tomcat to accept connections from outside localhost.

**What we did:**
```bash
sudo vi /etc/tomcat/server.xml
```

Removed this line:
```xml
address="127.0.0.1"
```

Final correct Connector:
```xml
<Connector port="3000"
           protocol="org.apache.coyote.http11.Http11NioProtocol"
           connectionTimeout="20000"
           redirectPort="8443" />
```

---

## 🔹 Step 9: Restarted Tomcat After Fix

**Why this step?**  
Configuration changes take effect only after restart.

```bash
sudo systemctl restart tomcat
```

---

## 🔹 Step 10: Verified Port Binding Again

**What we did:**
```bash
sudo netstat -tulnp | grep java
```

**Expected result:**
```
0.0.0.0:3000   LISTEN
```

This confirmed:  
Tomcat is now accessible from the network.

---

## 🔹 Step 11: Final Application Test

```bash
curl http://stapp01:3000
```

🎉 **Output received → Application working successfully.**

---

# 🎉 Final Outcome

- ✔ Tomcat installed correctly  
- ✔ Tomcat running on required port (3000)  
- ✔ Java application deployed successfully  
- ✔ Network binding issue fixed  
- ✔ Application accessible via base URL  

---

# 🧠 Key Learnings from This Task

- Service running does **NOT** mean service accessible  
- Always check IP + port binding  
- `127.0.0.1` restricts access to localhost  
- `0.0.0.0` allows external access  
- `netstat` is a powerful debugging tool  
- Restart services after configuration changes  

---

# 📝 Real DevOps Lesson

Always verify **where** a service is listening, not just whether it is running.



