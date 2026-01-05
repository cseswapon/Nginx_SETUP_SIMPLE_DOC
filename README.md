# VPS Setup & Node.js Environment Setup Guide

This guide provides step-by-step instructions to set up a **VPS server** (Ubuntu), and configure **Node.js**, **NVM**, **Git**, and other essential tools.

---

## Prerequisites

1. A **VPS** running **Ubuntu** or a similar Linux distribution.
2. **Root** or **sudo** access to the server.
3. Basic knowledge of using the Linux terminal.

---

## 1. Initial Server Setup

### Update and Upgrade System

It's important to ensure your system is up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2. Set File Permissions

Create a new file and set the appropriate permissions:

### Create a File

```bash
touch myfile.txt
```

### Change Permissions

```bash
chmod 644 myfile.txt
```

### Change Ownership (to a specific user)

```bash
sudo chown username:username myfile.txt
```

---

## 3. Basic Networking and Checking Running Ports

### List Open Ports

You can check which ports are being used by running:

```bash
sudo netstat -tuln
```

### Disable Unused Services

If you don't need certain services (e.g., Apache), disable them:

```bash
sudo systemctl stop apache2
sudo systemctl disable apache2
```

---

## 4. Installing NVM, Node.js, and Git

### Method 1: Installing Node.js via NVM (Recommended)

**NVM** (Node Version Manager) is the best way to manage Node.js versions.

#### Install NVM

Run this command to install NVM:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

#### Load NVM into Current Session

```bash
source ~/.bashrc
```

#### Install Node.js Using NVM

To install the latest LTS version of Node.js:

```bash
nvm install --lts
```

Or, install a specific version (e.g., v18):

```bash
nvm install 18
```

#### Verify Installation

Check the installed versions of **Node.js** and **npm**:

```bash
node -v
npm -v
```

---

### Installing Git

Install **Git** to use version control and clone repositories:

```bash
sudo apt install git -y
```

Verify Git installation:

```bash
git --version
```

---

## 5. Helpful Links

For detailed setup guides, troubleshooting, and configuration, refer to the following resources:

1. [Nginx + Node.js Setup](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_SETUP.md)
2. [Nginx + SSL + Certbot Setup](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_SSL_CERTBOT.md)
3. [Nginx Troubleshooting](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_TROUBLESHOOTING.md)
4. [Redis Setup](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/REDIS_SETUP.md)
5. [MongoDB Setup](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/VPS_MONGODB_SETUP.md)

---
