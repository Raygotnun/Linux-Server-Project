# Setting Up A WebServer with NGINX

## **Objectives**

The objective is to set up a web server capable of hosting a local webpage. This report focuses on the installation and configuration of an Nginx web server on a Debian system. Nginx is a high-performance HTTP server and reverse proxy, renowned for its stability, rich feature set, and low resource consumption. By deploying Nginx, we aim to create a robust platform for serving web content locally, which will lay the groundwork for understanding web server management and deployment in a Linux environment.

## **About NGINX**

Nginx is a high-performance, open-source web server known for its efficiency and scalability, commonly used for serving static files, reverse proxying, load balancing, and handling large amounts of concurrent traffic. Designed with an event-driven architecture, Nginx can manage thousands of simultaneous connections with low resource usage, making it ideal for high-traffic websites and applications. It supports various protocols, including HTTP, HTTPS, and TCP/UDP, and can easily integrate with applications and databases. Nginx is widely used to improve application speed and reliability, offering flexibility through modules and configuration options for different server roles.


---

## **Prerequisites**

- **Linux Server**: We use here a Debian-based server with root or sudo access.
- **Domain Name**: You should own local domain, (in this example **kamkar3project.com**) and have access to its DNS settings.

---

## **Step 1: Update the System**

First, update your package lists and upgrade existing packages.

```bash
sudo apt update
sudo apt upgrade -y
```

---

## **Step 2: Install Nginx**

Install Nginx using the apt package manager.

```bash
sudo apt install nginx -y
```

**Verify Nginx Installation:**

```bash
sudo systemctl status nginx
```

- **Expected Output:** Nginx should be active and running.

---

## **Step 3: Adjust Firewall Settings **

If you have a firewall enabled (like UFW), allow HTTP and HTTPS traffic.

**Check UFW Status:**

```bash
sudo ufw status
```

**Allow 'Nginx Full' Profile:**

```bash
sudo ufw allow 'Nginx Full'
```

**Enable UFW (if it's not enabled):**

```bash
sudo ufw enable
```

---

## **Step 4: Create a local DNS Record**

Point the domain **kamkar3project.com** to the server's IP address.

#### **Edit the Zone File**

```
sudo nano /etc/bind/db.kamkar3project.com
```

Add this configuration : 
```

;
; BIND data file for kamkar3project.com
;
$TTL    604800
@       IN      SOA     ns.kamkar3project.com. admin.kamkar3project.com. (
                              2         ; Serial (increment this after each change)
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.kamkar3project.com.
ns      IN      A       172.16.0.1
@       IN      A       172.16.0.1

```

---

## **Step 5: Create the Web Root Directory**

Create a directory to hold your website files.

```bash
sudo mkdir -p /var/www/kamkar3project.com/html
```

---

## **Step 6: Assign Ownership and Permissions**

Set the appropriate ownership and permissions for the web root directory.

```bash
sudo chown -R $USER:$USER /var/www/kamkar3project.com/html
sudo chmod -R 755 /var/www/kamkar3project.com
```

---

## **Step 7: Create your Web Page**

Create a simple HTML file to test your configuration.

```bash
nano /var/www/kamkar3project.com/html/index.html
```

Example of content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to kamkar3project.com!</title>
</head>
<body>
    <h1>Success! The Nginx server block is working!</h1>
</body>
</html>
```

**Save and exit** the file (`Ctrl + X`, then `Y`, and `Enter`).

---

## **Step 8: Create Nginx Server Block Configuration**

Set up Nginx to serve your domain.

```bash
sudo nano /etc/nginx/sites-available/kamkar3project.com
```

Add the following configuration:

```nginx
server {
    listen 80;
    listen [::]:80;

    root /var/www/kamkar3project.com/html;
    index index.html index.htm;

    server_name kamkar3project.com www.kamkar3project.com;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

**Explanation:**

- **listen 80;**: Listen on port 80 (HTTP).
- **root**: The web root directory.
- **server_name**: Your domain name.

---

## **Step 9: Enable the Server Block**

Activate your new server block by creating a symbolic link.

```bash
sudo ln -s /etc/nginx/sites-available/kamkar3project.com /etc/nginx/sites-enabled/
```

---

## **Step 10: Test Nginx Configuration**

Check for syntax errors.

```bash
sudo nginx -t
```

- **Expected Output:** Configuration file syntax is ok.

---

## **Step 11: Reload Nginx**

Apply the changes.

```bash
sudo systemctl reload nginx
```

---

## **Step 12: Verify the Website**

Open a web browser and navigate to:

```
http://kamkar3project.com
```

- **You should see** the sample web page created earlier.

---

