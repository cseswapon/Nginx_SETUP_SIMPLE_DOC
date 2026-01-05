

# Redis Setup Guide for Ubuntu

This guide explains how to install and configure **Redis** on an Ubuntu server and how to connect it to your **Node.js** application.

---

## Prerequisites

1. A server running **Ubuntu** or a similar Linux distribution.
2. **Root** or **sudo** privileges.
3. **Node.js** installed (optional, for Node.js integration).

---

## Step 1: Update Your System

Make sure your package lists are up-to-date:

```bash
sudo apt update
```

---

## Step 2: Install Redis

Install Redis server from the default Ubuntu repositories:

```bash
sudo apt install redis-server -y
```

---

## Step 3: Configure Redis (Optional)

### Edit Redis configuration file:

To allow Redis to listen for external connections (if needed), modify the Redis config file:

```bash
sudo nano /etc/redis/redis.conf
```

Find the line `bind 127.0.0.1 ::1` and change it to:

```bash
bind 0.0.0.0
```

This allows Redis to listen on all available network interfaces.

### Enable Redis to Start on Boot

To ensure Redis starts automatically after a reboot, enable the Redis service:

```bash
sudo systemctl enable redis-server
```

### Restart Redis to Apply Changes

```bash
sudo systemctl restart redis-server
```

---

## Step 4: Verify Redis Installation

Check the status of the Redis service to make sure it's running:

```bash
sudo systemctl status redis-server
```

You should see `active (running)` in the output.

### Test Redis

To test Redis, use the Redis CLI:

```bash
redis-cli
```

Once inside the Redis CLI, try the `ping` command:

```bash
127.0.0.1:6379> ping
```

It should return:

```bash
PONG
```

---

## Step 5: Integrating Redis with Node.js

### Install Redis Client for Node.js

To connect Redis to your **Node.js** app, install the `redis` package:

```bash
npm install redis
```

### Example: Connect to Redis from Node.js

Create a `redis-client.js` file to test the connection:

```javascript
const redis = require("redis");

// Connect to Redis server
const client = redis.createClient({
  host: '127.0.0.1',  // Redis server host
  port: 6379          // Redis default port
});

// Handle connection events
client.on('connect', function() {
  console.log('Connected to Redis...');
});

client.on('error', function(err) {
  console.log('Error: ' + err);
});

// Set and Get Redis values
client.set('my_key', 'Hello Redis!', redis.print);
client.get('my_key', function(err, reply) {
  console.log(reply);  // Should print "Hello Redis!"
});
```

Run the script:

```bash
node redis-client.js
```

You should see the connection message and the value stored in Redis.

---

## Step 6: Enable Redis Persistence (Optional)

By default, Redis does not persist data. You can configure Redis to persist data by adjusting the `appendonly` configuration in `/etc/redis/redis.conf`.

Edit the Redis config file:

```bash
sudo nano /etc/redis/redis.conf
```

Find the following line:

```bash
appendonly no
```

Change it to:

```bash
appendonly yes
```

This enables **Append Only File** persistence, which logs every write operation.

---

## Step 7: Redis Security (Optional)

### Set a Password for Redis

To set a password for Redis to restrict unauthorized access, add the following to `/etc/redis/redis.conf`:

```bash
requirepass your_redis_password
```

Restart Redis to apply changes:

```bash
sudo systemctl restart redis-server
```

---

## Step 8: Monitor Redis (Optional)

### Check Redis Memory Usage

Use the `INFO` command to view Redis stats:

```bash
redis-cli info memory
```

This will show memory usage statistics.

---

## Troubleshooting

If Redis is not working as expected, here are a few steps to troubleshoot:

1. **Check Redis logs:**
   View Redis logs for any errors:

   ```bash
   sudo tail -f /var/log/redis/redis-server.log
   ```

2. **Check Redis status:**
   Make sure the Redis service is active:

   ```bash
   sudo systemctl status redis-server
   ```

3. **Check for network/firewall issues:**
   If you're connecting to Redis from an external server, ensure that the Redis port (default: 6379) is not blocked by a firewall.

   Allow Redis through the firewall:

   ```bash
   sudo ufw allow 6379
   sudo ufw reload
   ```

---

## Final Thoughts

Congratulations! You have successfully installed and configured Redis on your server and integrated it with your **Node.js** application. 🎉

---

