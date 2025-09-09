# Setting Up SSL with Certbot for Nginx + Node.js

This guide explains how to secure your Node.js Express application with **Let's Encrypt SSL certificates** using **Certbot** and how to automatically renew them.

---

## Prerequisites

1. A server running Ubuntu or similar Linux distribution.
2. Nginx installed and configured as a reverse proxy.
3. A domain name pointing to your server's public IP (e.g., `your-domain.com`).
4. Node.js app running (e.g., on port `5000`).

---

## Step 1: Install Certbot

Run the following commands to install Certbot and the Nginx plugin:

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```

---

## Step 2: Obtain an SSL Certificate

Request a free Let's Encrypt SSL certificate for your domain:

```bash
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```

* Replace `your-domain.com` with your actual domain.
* Certbot will automatically configure Nginx for HTTPS.
* You will be prompted for an email (for renewal notices) and to agree to the terms.

After success, you will see:

```
Congratulations! Your certificate and chain have been saved.
Your certificate will expire on ...
```

---

## Step 3: Verify HTTPS

Open your browser and visit:

```
https://your-domain.com
```

You should see your Node.js app served securely over HTTPS.

---

## Step 4: Auto Renewal Setup

Certbot sets up auto‑renewal by default. To check if renewal works, run:

```bash
sudo certbot renew --dry-run
```

If successful, renewal is properly configured.

---

## Step 5: Renewal Cron Job (Optional)

Certbot usually installs a systemd timer. To double‑check, you can add a cron job manually:

```bash
sudo crontab -e
```

Add this line:

```bash
0 3 * * * certbot renew --quiet && systemctl reload nginx
```

This will run every day at 3 AM to renew certificates if needed and reload Nginx.

---

## Step 6: Check Certificate Expiry

To check when your certificate expires:

```bash
sudo certbot certificates
```

You’ll see details like domain names, expiry dates, and file locations.

---

## Troubleshooting

* Check Nginx error logs:

  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```
* Check Certbot logs:

  ```bash
  sudo less /var/log/letsencrypt/letsencrypt.log
  ```
* Verify firewall rules (allow HTTPS):

  ```bash
  sudo ufw allow 'Nginx Full'
  sudo ufw reload
  ```

---

