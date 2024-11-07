

```markdown
# Setting Up SSH on a Linux Server

This document outlines the steps I took to set up SSH on my Linux server, including security configurations for safe remote access.

```

## Step 1: Install the OpenSSH Server

To start, I needed to install the OpenSSH server package to allow SSH connections:

```bash
sudo apt update
sudo apt install openssh-server -y
```To confirm the installation, I checked if the SSH service was running:

```bash
sudo systemctl status ssh
```

If the service was active, then the SSH server was successfully installed and running.


## Step 2: Test SSH Access

Finally, I tested the SSH setup by connecting from my local machine:

```bash
ssh myuser@server_ip
```


Once connected, I confirmed that everything was set up correctly.

## Conclusion

Following these steps, I successfully set up SSH on my Linux server with enhanced security. By using key-based authentication, a custom port, and restricted user access, I ensured that my server was both accessible and secure.
```

This document guides through setting up SSH on a Linux server, emphasizing security configurations for safe access.
