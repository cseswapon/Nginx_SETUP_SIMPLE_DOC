# Nginx Configuration Troubleshooting and Solution Guide

This guide describes the issues faced while setting up Nginx with multiple configurations and their corresponding solutions. It also covers using Node.js on the server side effectively.

---

## Problem: Conflicting Nginx Configuration Files

### Configuration Files

#### `/etc/nginx/sites-available/node_project`
```nginx
server {
    listen 74;

    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

#### `/etc/nginx/sites-available/default`
```nginx
server {
    listen 99;

    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Issues:
1. **Port Conflicts**: Using different ports (`74` and `99`) in the configuration files can cause conflicts if not properly managed.
2. **Node.js Server Already Running**: If a Node.js server is already running on the same port, Nginx cannot bind to it.
3. **Duplicate Configurations**: Two configurations pointing to the same backend (`http://localhost:5000`) can cause unexpected behavior.
4. **Symbolic Link Errors**: Attempting to enable a configuration file when a symbolic link already exists results in an error.

---

## Solution: Unified Configuration and Deployment

### Step 1: Consolidate Nginx Configuration Files
Merge the configurations into a single file to avoid conflicts:

```nginx
server {
    listen 80;

    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Save this configuration to `/etc/nginx/sites-available/node_project`.

### Step 2: Remove Duplicate Links
If a symbolic link already exists, remove it before creating a new one:
```bash
sudo rm /etc/nginx/sites-enabled/node_project
```

Create a new symbolic link:
```bash
sudo ln -s /etc/nginx/sites-available/node_project /etc/nginx/sites-enabled/
```

### Step 3: Test and Reload Nginx
Validate the Nginx configuration:
```bash
sudo nginx -t
```

Reload Nginx to apply changes:
```bash
sudo systemctl reload nginx
```

### Step 4: Ensure Node.js Server is Running
Start your Node.js server on port `5000`. Use a process manager like PM2 for better management:
```bash
pm install -g pm2
pm2 start app.js
```

---

## Additional Troubleshooting

### Check Active Nginx Configuration
List enabled configurations:
```bash
ls -l /etc/nginx/sites-enabled/
```

### Check Logs
- Nginx error logs:
  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```
- Node.js logs:
  ```bash
  pm2 logs
  ```

### Verify Port Usage
Check which processes are using ports:
```bash
sudo netstat -tuln | grep ':80'
```

---

## Final Verification
1. Access your application via the server's IP or domain (e.g., `http://your-domain.com`).
2. Ensure Node.js is properly proxied by Nginx.

---

You now have a streamlined Nginx setup for your Node.js application. 🎉
