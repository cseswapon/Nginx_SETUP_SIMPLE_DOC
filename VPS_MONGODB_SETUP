# 🚀 MongoDB Setup Guide (VPS / Ubuntu)

This guide explains how to install, secure, and configure **MongoDB 8.0** on an Ubuntu VPS step by step.

---

## 📌 Prerequisites

* Ubuntu 22.04 (Jammy)
* Root or sudo access
* Internet connection

---

## 📦 Install MongoDB

### 1️⃣ Install Required Packages

```bash
sudo apt-get install gnupg curl
```

### 2️⃣ Add MongoDB GPG Key

```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor
```

### 3️⃣ Add MongoDB Repository

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```

### 4️⃣ Update & Install MongoDB

```bash
sudo apt-get update
sudo apt-get install -y mongodb-org
```

---

## ▶️ Start & Enable MongoDB Service

```bash
sudo systemctl daemon-reload
sudo systemctl start mongod
sudo systemctl enable mongod
```

### ✅ Verify MongoDB Status

```bash
sudo systemctl status mongod
```

---

## 🔗 Default MongoDB Connection URI

```text
mongodb://127.0.0.1:27017
```

---

## 🔐 (Optional) Configure MongoDB Security & Authentication

### 🔥 Configure Firewall (UFW)

Enable firewall:

```bash
sudo ufw enable
```

Check firewall status:

```bash
sudo ufw status
```

Allow MongoDB & SSH ports:

```bash
sudo ufw allow 27017
sudo ufw allow 'OpenSSH'
```

Restart MongoDB:

```bash
sudo service mongod restart
```

---

## 👤 Create MongoDB Super User (Admin)

### 1️⃣ Open Mongo Shell

```bash
mongosh
```

### 2️⃣ Switch to Admin Database

```bash
use admin
```

### 3️⃣ Create Root User

```bash
db.createUser({
  user: "username-here",
  pwd: passwordPrompt(),
  roles: ["root"]
})
```

Exit shell:

```bash
.exit
```

---

## 🔒 Enable MongoDB Authorization

Edit MongoDB config file:

```bash
sudo nano /etc/mongod.conf
```

Uncomment / add:

```yaml
security:
  authorization: enabled
```

Restart MongoDB:

```bash
sudo service mongod restart
```

---

## 🔑 Login as Super User

```bash
mongosh --port 27017 --authenticationDatabase "admin" -u "username-here" -p "password-here"
```

---

## 🗄️ Create Database & Project User

```bash
use database_name
```

```bash
db.createUser({
  user: "username_here",
  pwd: passwordPrompt(),
  roles: [{ role: "readWrite", db: "database_name" }]
})
```

Exit shell:

```bash
.exit
```

---

## 🔗 MongoDB URI for Project Connection

```text
mongodb://username-here:password-here@127.0.0.1:27017/database_name
```

---
