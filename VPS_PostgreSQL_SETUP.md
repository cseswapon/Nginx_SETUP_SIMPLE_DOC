# 🐘 PostgreSQL Setup Guide (User & Password)

This document describes a **clean and secure PostgreSQL setup** on an **Ubuntu VPS**, including database creation, user management, and password configuration for production use.

---

## 📌 System Requirements

* Ubuntu 20.04 / 22.04 LTS
* Root or sudo access

---

## 🔹 1. Install PostgreSQL

Update package list and install PostgreSQL:

```bash
sudo apt update
sudo apt install -y postgresql postgresql-contrib
```

Verify service status:

```bash
sudo systemctl status postgresql
```

---

## 🔹 2. Switch to PostgreSQL Default User

PostgreSQL creates a default system user called `postgres`.

```bash
sudo -i -u postgres
```

Access PostgreSQL shell:

```bash
psql
```

---

## 🔹 3. Create Database

```sql
CREATE DATABASE myapp_db;
```

Verify:

```sql
\l
```

---

## 🔹 4. Create Database User with Password

```sql
CREATE USER myapp_user WITH PASSWORD 'strong_password_here';
```

> 🔐 Use a **strong password** (minimum 12 characters recommended).

---

## 🔹 5. Grant Privileges

Grant full access to the database:

```sql
GRANT ALL PRIVILEGES ON DATABASE myapp_db TO myapp_user;
```

(Optional – recommended for app migrations)

```sql
ALTER USER myapp_user CREATEDB;
```

---

## 🔹 6. Change Ownership (Optional but Recommended)

```sql
ALTER DATABASE myapp_db OWNER TO myapp_user;
```

---

## 🔹 7. Exit PostgreSQL

```sql
\q
exit
```

---

## 🔹 8. Enable Password Authentication (Production)

Edit PostgreSQL configuration:

```bash
sudo nano /etc/postgresql/*/main/pg_hba.conf
```

Change this line:

```conf
local   all             all                                     peer
```

To:

```conf
local   all             all                                     md5
```

Restart PostgreSQL:

```bash
sudo systemctl restart postgresql
```

---

## 🔹 9. Test Login with New User

```bash
psql -U myapp_user -d myapp_db -h 127.0.0.1
```

---

## 🔹 10. PostgreSQL Connection String (Node.js)

Use this format in your `.env` file:

```env
DATABASE_URL=postgresql://myapp_user:strong_password_here@localhost:5432/myapp_db
```

Example (pg / Prisma / Sequelize compatible):

```js
postgresql://user:password@host:port/database
```

---

## 🔹 11. Basic Security Best Practices

* ❌ Never expose PostgreSQL to public internet
* ✅ Bind PostgreSQL to `localhost`
* ✅ Use `.env` for credentials
* ✅ Restrict DB user permissions per app

Check bind address:

```bash
sudo nano /etc/postgresql/*/main/postgresql.conf
```

Ensure:

```conf
listen_addresses = 'localhost'
```

Restart service:

```bash
sudo systemctl restart postgresql
```
