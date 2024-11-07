<img src="assets/Dalibrary.webp">
# Linux-Server-Project

Welcome to the Linux Server Project! In this project, we aim to set up a variety of network services on a Linux-based server, including **DHCP**, **DNS**, **Web Server (Nginx)**, **Cron jobs**, and **SSH**. The goal is to provide a fully functional server environment where all these services work in harmony to support a small-scale network.

## Project Overview

In this project, we set up and configured multiple network services on a Linux server, including:

- **DHCP Server** for dynamic IP address allocation
- **DNS Server** for local name resolution
- **Web Server (Nginx)** to serve web pages
- **Cron Jobs** for scheduling tasks
- **SSH Server** for secure remote login

The project also involves troubleshooting and configuring these services, ensuring that they work properly, and learning the intricacies of Linux system administration.

---

## Network Design Summary

### Overview

The project simulates a server environment in a NAT network where we configure **DHCP**, **DNS**, **Web server (Nginx)**, **SSH**, and **Cron jobs**. The project is designed to demonstrate how to efficiently configure these services on a Linux machine to handle typical network and administrative tasks.

---

**IP Table**

| Device             | IP Address  |
|--------------------|-------------|
| **Server (DHCP/DNS)** | `10.0.2.15/24`   |
| **Client**          | `DHCP Assigned`   |

---

### Network Devices

- **Linux Server**: Configured with multiple network services.
- **Client Machine**: Configured to receive IP dynamically via DHCP.


---

### Key Services Configured
**[1. DHCP Server](https://github.com/Raygotnun/Linux-Server-Project)**
- **Service**: The DHCP server was configured to assign dynamic IP addresses to clients on the local network.
  
**[2. DNS Server (BIND)](https://github.com/Raygotnun/Linux-Server-Project)**
- **Service**: The DNS server (BIND9) was configured to resolve local domain names (`library.local`) to IP addresses.
  
**[3. Web Server (Nginx)](https://github.com/Raygotnun/Linux-Server-Project)**
- **Service**: The Nginx web server was set up to serve static pages and allow remote access via HTTP.
  
**[4. Cron Jobs](https://github.com/Raygotnun/Linux-Server-Project)**
- **Service**: Cron jobs were used to automate scheduled tasks on the server.

**[5. SSH Server](https://github.com/Raygotnun/Linux-Server-Project)**
- **Service**: SSH (Secure Shell) was configured to allow remote connections to the server.

---

## How Everything Works

1. **DHCP Server** assigns dynamic IP addresses to client devices, allowing them to communicate on the local network.
2. **DNS Server** resolves domain names (e.g., `library.local`) to IP addresses for clients and other network devices.
3. **Web Server (Nginx)** provides HTTP services, allowing users to access a basic web page hosted on the server.
4. **Cron Jobs** automate tasks like backups or system maintenance to run at scheduled intervals.
5. **SSH** allows secure remote login, enabling administration and troubleshooting of the server from anywhere in the network.

---

## Challenges Faced

1. **DHCP Server Misconfiguration**: 
   - The DHCP service was initially not assigning IP addresses correctly due to misconfiguration in `/etc/dhcp/dhcpd.conf` and the DHCP daemon not being properly restarted. The `dhcpd.leases` file was checked to confirm if leases were being assigned.

2. **DNS Resolution Issues**:
   - DNS queries were timing out because the DNS server (BIND) was not properly listening on the correct IP interfaces. The server was configured to listen on `10.0.2.15`, but DNS queries were not being routed correctly. This required ensuring that the BIND service was configured to listen on the correct network interface.

3. **Firewall Blockages**:
   - The firewall was blocking essential ports (e.g., HTTP, DNS, SSH) during testing. This was resolved by temporarily disabling the firewall to confirm that the services were functioning properly.

4. **Network Configuration Challenges**:
   - The project involved troubleshooting network configurations, including ensuring that the client VM could communicate with the server through NAT and that proper DNS resolution was functioning.

5. **Service Restarts**:
   - Ensuring services (e.g., `named`, `dhcpd`, `nginx`) were properly restarted after configuration changes was a recurring challenge that required close attention to service status.

---

## How to Use This Project

1. **Clone the Repository**:
   - Download or clone this repository to your local machine.
   
2. **Server Configuration**:
   - Follow the instructions to configure the **DHCP**, **DNS**, **Web Server**, **SSH**, and **Cron** services on your Linux server.

3. **Verify Connectivity**:
   - Use `ping`, `dig`, `curl`, and `ssh` to verify that the services are functioning:
     - Test **DHCP** by checking that the client receives an IP.
     - Test **DNS** by resolving domain names with `dig @localhost library.local`.
     - Test **Web Server** by accessing `http://<server-ip>` from a browser.
     - Test **SSH** by connecting remotely using `ssh user@<server-ip>`.

4. **Test Cron Jobs**:
   - Check that automated tasks run correctly according to the cron schedule.

---

## Contributors

- **Name** - put the damn roles
- **Stephane Paris** - Configuration & Documention

---

## Future Improvements

1. **Scalability**: The project could be expanded to support more clients, additional network services, or more complex network configurations.
2. **Security Enhancements**: Add advanced firewall configurations, IPsec, or VPN to secure communication across the network.
3. **Monitoring**: Integrate monitoring tools (e.g., Nagios, Zabbix) to track the health of the server and network services.
4. **Backup Solutions**: Automate backups with cron jobs and implement a robust backup system for critical data.

--- 
