# Deploy Static Website on Nginx (codersdiary.shop)

Deploying a static website on a **Nginx web server** is a simple, efficient, and interview-friendly approach.

This guide provides **step-by-step instructions** to deploy the `codersdiary.shop` static website on a **Linux VM / AWS EC2 (Ubuntu)**.

---

## 🏗️ Architecture (Simple)

```
GitHub Repository  →  LinuxServer(EC2 / VM)  →  Nginx  →  Browser

```

---

## ✅ Prerequisites

- Linux server (AWS EC2 or any VM with Ubuntu)
- Port **80** open in Security Group / Firewall
- SSH access to the server
- Domain name (optional but recommended)

---

## 🔹 STEP 1: Connect to Your Server

```bash
ssh ubuntu@<SERVER_PUBLIC_IP>

```

---

## 🔹 STEP 2: Install Nginx

```bash
sudo apt update
sudo apt install nginx -y

```

### Check Nginx Status

```bash
sudo systemctl status nginx

```

### Verify in Browser

```
http://<SERVER_PUBLIC_IP>

```

You should see the **Welcome to Nginx** page.

---

## 🔹 STEP 3: Install Git

```bash
sudo apt install git -y

```

---

## 🔹 STEP 4: Clone GitHub Repository

```bash
cd /var/www
sudo gitclone https://github.com/Shubham-6364/codersdiary.shop.git

```

Website files location:

```bash
/var/www/codersdiary.shop

```

---

## 🔹 STEP 5: Set Correct Permissions

```bash
sudochown -R www-data:www-data /var/www/codersdiary.shop
sudochmod -R 755 /var/www/codersdiary.shop

```

---

## 🔹 STEP 6: Create Nginx Server Block

Create a new server block file:

```bash
sudo nano /etc/nginx/sites-available/codersdiary.shop

```

### Paste the following configuration:

```
server {
listen80;
server_name codersdiary.shop www.codersdiary.shop;

root /var/www/codersdiary.shop;
index index.html;

location / {
try_files$uri$uri/ =404;
    }
}

```

Save and exit.

---

## 🔹 STEP 7: Enable the Site

Enable the configuration:

```bash
sudoln -s /etc/nginx/sites-available/codersdiary.shop /etc/nginx/sites-enabled/

```

Remove default site (important):

```bash
sudorm /etc/nginx/sites-enabled/default

```

Test Nginx configuration:

```bash
sudo nginx -t

```

Reload Nginx:

```bash
sudo systemctl reload nginx

```

---

## 🔹 STEP 8: DNS Configuration (Domain Setup)

Add the following DNS records in your domain provider:

| Type | Name | Value |
| --- | --- | --- |
| A | @ | SERVER_PUBLIC_IP |
| A | www | SERVER_PUBLIC_IP |

Wait **5–10 minutes** for DNS propagation.

Open:

```
http://codersdiary.shop

```

---

## 🔹 STEP 9 (Optional but Recommended): Enable HTTPS (SSL)

Install Certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y

```

Generate SSL certificate:

```bash
sudo certbot --nginx -d codersdiary.shop -d www.codersdiary.shop

```

Test auto-renewal:

```bash
sudo certbot renew --dry-run

```

---

## 🔹 STEP 10: Update Site Automatically (CI/CD Lite)

Whenever you push changes to GitHub:

```bash
cd /var/www/codersdiary.shop
sudo git pull
sudo systemctl reload nginx

```

> This can be further automated using GitHub Actions for full CI/CD.
> 

---

## 📌 Summary

- Static website deployed using **Nginx**
- GitHub used as source control
- Custom domain configured
- HTTPS enabled using **Certbot**
- Lightweight CI/CD via `git pull`

---

## 👨‍💻 Author

**Shubham**

DevOps Engineer

GitHub: [Shubham-6364](https://github.com/Shubham-6364)
