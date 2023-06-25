---
sidebar_position: 1
---

# Nginx

**How to setup a Virtual Host for Nginx server.**

## Prerequisite:

- [Nginx](https://www.nginx.com/resources/wiki/start/topics/tutorials/install) is installed on the server.
- An existing server that is running, e.g: An Node.js API backend server running at `http://localhost:3000`.
- A registered domain, e.g: `ecommerce.com`.

## This article assumes that will:

- Create a Virtual Host on Nginx to route traffic for `ecommerce.com` to the existing `http://localhost:3000` process in that same server.
- Force HTTPS by auto-redirecting `http://ecommerce.com` to `https://ecommerce.com`.
- Remove www prefix by auto-redirecting `www.ecommerce.com` to `https://ecommerce.com`.
- Use gzip to compress server responses.

### Step 1: Create `/etc/nginx/sites-available/ecommerce.com file`

```
server {
        server_name www.ecommerce.com;
        return 301 http://ecommerce.com$request_uri;
}

server {
        listen 80;
        server_name ecommerce.com;

        gzip on;

        location / {
                proxy_pass http://localhost:3000;
                proxy_set_header Host $http_host;
                proxy_set_header X-Forwarded-Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}
```

### Step 2: Test and restart Nginx to apply new config

```bash
sudo ln -s /etc/nginx/sites-available/ecommerce.com /etc/nginx/sites-enabled
```

```bash
#Test Nginx config
sudo nginx -t
```

```bash
# Restart Nginx
sudo systemctl restart nginx
```

```bash
# Or:
sudo service nginx restart
```

### Step 3: Additional step to support HTTPS

**As a further step, we will:**

- Force HTTPS by auto-redirecting `http://ecommerce.com` to `https://ecommerce.com`.
- Remove www prefix by auto-redirecting `www.ecommerce.com` to `https://ecommerce.com`.

```
server {
        listen 80;
        server_name ecommerce.com;
        return 301 https://$host$request_uri;
}

server {
        server_name www.ecommerce.com;
        return 301 https://ecommerce.com$request_uri;
}

server {
        listen 443 ssl;
        server_name ecommerce.com;

        ssl_certificate /etc/nginx/ssl/_.ecommerce.com_ssl_certificate_bundle.cer;
        ssl_certificate_key /etc/nginx/ssl/_.ecommerce.com.key;

        gzip on;

        location / {
                proxy_pass http://localhost:3000;
                proxy_set_header Host $http_host;
                proxy_set_header X-Forwarded-Host $http_host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}
```

**Restart Nginx to apply new config.**
