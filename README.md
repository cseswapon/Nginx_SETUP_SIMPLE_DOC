# Setting Up Nginx with a Node.js Express Project

This guide explains how to set up Nginx as a reverse proxy for a Node.js Express application and ensure no conflicts with other servers like Apache.

---

## Prerequisites

1. A server running Ubuntu or a similar Linux distribution.
2. Node.js and npm installed.
3. Nginx installed.
4. An Express.js project ready to deploy.

---

## Step 1: Stop and Disable Conflicting Servers

If other services like Apache are running, stop and disable them to free up port 80:
 
```bash
sudo systemctl stop apache2
sudo systemctl disable apache2
```

To check which service is using port 80:

```bash
sudo netstat -tuln | grep ':80'
```

---

## Step 2: Install and Start Nginx

1. Install Nginx if not already installed:
   ```bash
   sudo apt update
   sudo apt install nginx
   ```

2. Enable and start Nginx:
   ```bash
   sudo systemctl enable nginx
   sudo systemctl start nginx
   ```

3. Check Nginx status:
   ```bash
   sudo systemctl status nginx
   ```

---

## Step 3: Configure Nginx as a Reverse Proxy

1. Open a new Nginx configuration file:
   ```bash
   sudo nano /etc/nginx/sites-available/node_project
   ```

2. Add the following configuration to set up Nginx as a reverse proxy for your Node.js app:
   ```nginx
   server {
       listen 80;

       server_name your-domain.com;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```
   Replace `your-domain.com` with your domain or server's IP address and `3000` with the port your Node.js app is running on.

3. Enable the configuration:
   ```bash
   sudo ln -s /etc/nginx/sites-available/node_project /etc/nginx/sites-enabled/
   ```

4. Test the Nginx configuration:
   ```bash
   sudo nginx -t
   ```

5. Reload Nginx:
   ```bash
   sudo systemctl reload nginx
   ```

---

## Step 4: Start Your Node.js Express App

1. Start your Node.js app:
   ```bash
   node app.js
   ```
   Or use a process manager like PM2 for better app management:
   ```bash
   pm2 start app.js
   ```

2. Ensure your app is running on the port specified in the Nginx configuration (e.g., 3000).

---

## Step 5: Verify the Setup

1. Open your browser and navigate to your server's domain or IP address (e.g., `http://your-domain.com`).
2. You should see your Node.js application served through Nginx.

---

## Troubleshooting

- Check Nginx logs for errors:
  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```
- Check your Node.js app logs if it's not responding:
  ```bash
  pm2 logs
  ```
- Ensure the firewall allows HTTP traffic:
  ```bash
  sudo ufw allow 'Nginx Full'
  sudo ufw reload
  ```

---

## Optional: Secure Nginx with SSL (HTTPS)

1. Install Certbot:
   ```bash
   sudo apt install certbot python3-certbot-nginx
   ```

2. Obtain an SSL certificate:
   ```bash
   sudo certbot --nginx -d your-domain.com
   ```

3. Verify SSL setup:
   ```bash
   https://your-domain.com
   ```

---

You're now ready to serve your Node.js Express app with Nginx!

## PRODUCTION BUILD .CJS

```bash
   const express = require("express");
   const app = express();

   // serve up production assets
   const path = require("path");
   app.use(express.static(path.join(__dirname, "/")));

   // let the react app to handle any unknown routes
   // serve up the index.html if express does'nt recognize the route
   app.get("*", (req, res) => {
      res.sendFile(path.resolve(__dirname, "index.html"));
   });

   // if not in production use the port 5000
   const PORT = 5000;

   // console.log("server started on port:", PORT);
   app.listen(PORT);
```
