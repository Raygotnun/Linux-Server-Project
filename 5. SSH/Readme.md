# Secure Access with SSH


## Overview

SSH (Secure Shell) is a cryptographic network protocol used on Linux systems for secure remote login and command execution over unsecured networks. It enables users to access and manage remote servers securely by encrypting the connection between the client and the server. Typically, OpenSSH is the most common implementation on Linux, providing tools like ssh for logging into remote machines, scp for secure file copying, and sftp for secure FTP. To initiate a connection, users employ the ssh command followed by the remote username and host (ssh user@hostname). SSH supports key-based authentication, enhancing security by using public and private key pairs instead of passwords. This protocol is essential for system administration, remote support, and securely managing servers and applications.

---


## **Prerequisites**

- **Operating Systems**: Both the server and client should be running Linux (Debian-based) or have SSH capabilities.
- **User Permissions**: You should have sudo or root access on both machines.
- **Network Connectivity**: Ensure both machines can communicate over the network (check firewalls and routers).

---

## **Step 1: Install OpenSSH Server and Client**

### **On Both Server and Client Machines**

1. **Update Package Lists:**

   ```bash
   sudo apt update
   ```

2. **Install OpenSSH Server and Client:**

   ```bash
   sudo apt install openssh-server openssh-client -y
   ```

3. **Check SSH Service Status:**

   ```bash
   sudo systemctl status ssh
   ```

   - If it's not running, start and enable the SSH service:

     ```bash
     sudo systemctl start ssh
     sudo systemctl enable ssh
     ```

---

## **Step 2: Configure SSH Server Settings**

### **On Both Machines**

1. **Edit SSH Configuration File:**

   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. **Basic Configuration Options:**

   - **Change Default Port (Optional for Security):**

     ```bash
     Port 22  # Default is 22; change to a non-standard port if desired
     ```

   - **Disable Root Login (Recommended):**

     ```bash
     PermitRootLogin no
     ```

   - **Allow Specific Users (Optional):**

     ```bash
     AllowUsers your_username
     ```

3. **Save and Exit:**

   - In `nano`, press `Ctrl + X`, then `Y`, and `Enter`.

4. **Restart SSH Service to Apply Changes:**

   ```bash
   sudo systemctl restart ssh
   ```

---

## **Step 3: Adjust Firewall Settings**

### **On Both Machines**

1. **Check if UFW (Uncomplicated Firewall) is Installed:**

   ```bash
   sudo ufw status
   ```

2. **Allow SSH Connections Through the Firewall:**

   - **Default Port (22):**

     ```bash
     sudo ufw allow ssh
     ```

   - **Custom Port (if you changed it):**

     ```bash
     sudo ufw allow <your_custom_port>/tcp
     ```

3. **Enable UFW (if not already enabled):**

   ```bash
   sudo ufw enable
   ```

4. **Reload UFW to Apply Changes:**

   ```bash
   sudo ufw reload
   ```

---

## **Step 4: Generate SSH Key Pairs**

### **On Both Machines**

1. **Generate SSH Key Pair:**

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   - **When prompted:**
     - **Enter file in which to save the key:** Press `Enter` to accept the default location (`/home/your_username/.ssh/id_rsa`).
     - **Enter passphrase (optional but recommended):** Enter a secure passphrase or leave it empty for no passphrase.
     - **Enter same passphrase again:** Re-enter your passphrase.

2. **This will create two files:**

   - **Private Key:** `~/.ssh/id_rsa`
   - **Public Key:** `~/.ssh/id_rsa.pub`

---

## **Step 5: Exchange Public Keys**

### **From Client to Server (Allows Client to SSH into Server)**

1. **Copy Client's Public Key to Server:**

   On the **client machine**, run:

   ```bash
   ssh-copy-id your_username@server_ip_address
   ```

   - **Replace `your_username` with your username on the server.**
   - **Replace `server_ip_address` with the server's IP address.**

2. **If Prompted, Accept the Server's Fingerprint:**

   - Type `yes` and press `Enter`.

3. **Enter Your Password:**

   - Provide the password for `your_username` on the server.

### **From Server to Client (Allows Server to SSH into Client)**

1. **Copy Server's Public Key to Client:**

   On the **server machine**, run:

   ```bash
   ssh-copy-id your_username@client_ip_address
   ```

   - **Replace `your_username` with your username on the client.**
   - **Replace `client_ip_address` with the client's IP address.**

2. **If Prompted, Accept the Client's Fingerprint:**

   - Type `yes` and press `Enter`.

3. **Enter Your Password:**

   - Provide the password for `your_username` on the client.

---

## **Step 6: Test SSH Connections**

### **From Client to Server**

1. **On the Client Machine:**

   ```bash
   ssh your_username@server_ip_address
   ```

2. **You Should Be Logged Into the Server Without a Password (if passphrase was not set).**

### **From Server to Client**

1. **On the Server Machine:**

   ```bash
   ssh your_username@client_ip_address
   ```

2. **You Should Be Logged Into the Client Without a Password (if passphrase was not set).**

