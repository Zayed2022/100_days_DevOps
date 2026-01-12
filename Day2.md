# Day 2: Temporary User Setup with Expiry

## 📋 Task Overview
As part of the Nautilus project, a temporary user account needs to be created for a developer named **javed**. To maintain security and access management standards, the account must be configured with a specific expiration date.

### Task Specifications:
- **Username:** `javed`
- **Server:** `App Server 2` (stapp02)
- **Expiration Date:** `2027-04-15`
- **Protocol:** Username must be lowercase.

---

## 🚀 Implementation Steps

### 1. Connect to the Server
Access the Nautilus infrastructure via the Jump Host and SSH into **App Server 2**.
```bash
ssh steve@stapp02

```

### 2. Create the User with Expiry Date

Execute the `useradd` command with the `-e` flag. This flag sets the date on which the user account will be disabled. The date format used is `YYYY-MM-DD`.

```bash
sudo useradd -e 2027-04-15 javed

```

### 3. Verify the Account Expiry

To confirm the task was completed successfully, check the account aging details using the `chage` (change age) command.

```bash
sudo chage -l javed

```

**Verification Output:**
The command should return a line stating:
`Account expires : Apr 15, 2027`

---

## 🧠 Technical Notes

* **Command Used:** `useradd`
* **Flag `-e`:** Used for account expiration. This is different from password expiration; the entire account becomes inaccessible after this date.
* **Security Best Practice:** Setting expiry dates for temporary contractors or short-term developers prevents "zombie accounts" from remaining active in the infrastructure indefinitely.

---

```
