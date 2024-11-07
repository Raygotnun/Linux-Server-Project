





Here's are the details for setting up a DHCP server on a Linux (Ubuntu) server:

```markdown
# How I Set Up a DHCP Server on a Linux Server

Here’s the step-by-step process I followed to set up a DHCP server on my Linux server.

---

## Step 1: System Preparation

First, I made sure my system was up to date. This keeps everything running smoothly and avoids compatibility issues:

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Installing DHCP Server Software

I used `isc-dhcp-server` since it’s reliable and widely used. To install it, I ran:

```bash
sudo apt install isc-dhcp-server -y
```

## Step 3: Configuring DHCP Settings

The main configuration file for the DHCP server is located at `/etc/dhcp/dhcpd.conf`. I edited this file to define the IP address range and network options.

### Editing the Configuration File

To open the configuration file, I used:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Inside the file, I configured the DHCP settings to suit my network. Below is the configuration I added (adjusted to my network setup):

```conf
# Basic DHCP server settings

# Domain name for clients (optional)
option domain-name "example.com";

# DNS servers for the network
option domain-name-servers ns1.example.com, ns2.example.com;

# Lease times in seconds
default-lease-time 600;
max-lease-time 7200;

# Subnet configuration
subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;
    option broadcast-address 192.168.10.255;
}
```

- **Range**: I set this to the pool of IP addresses that can be assigned to clients.
- **Routers**: Set the gateway IP.
- **Broadcast-address**: Defined the broadcast address for the network.

### Setting the Network Interface

Next, I configured which network interface would handle DHCP requests. I opened the DHCP defaults file:

```bash
sudo nano /etc/default/isc-dhcp-server
```

In this file, I specified my network interface (`eth0` in my case) by editing the `INTERFACESv4` line like so:

```conf
INTERFACESv4="eth0"
```

To find the correct interface name, I ran `ip a` to list all network interfaces.

## Step 4: Starting and Enabling the DHCP Service

Once the configuration was set, I started the DHCP service:

```bash
sudo systemctl start isc-dhcp-server
```

Then I enabled it to start automatically on system boot:

```bash
sudo systemctl enable isc-dhcp-server
```

## Step 5: Verifying DHCP Status

To confirm everything was working, I checked the status of the DHCP server:

```bash
sudo systemctl status isc-dhcp-server
```

If the service was active, that meant the DHCP server was running. 

## Step 6: Troubleshooting (If Necessary)

For any issues, I checked the system logs to troubleshoot:

```bash
sudo tail -f /var/log/syslog
```

The log usually provided details on what might be wrong with the configuration.


This configuration reserves `192.168.1.150` for a device with MAC address `00:11:22:33:44:55`.

## Conclusion

After completing these steps, my DHCP server was set up on Linux and started assigning IP addresses to clients on my network. Testing confirmed that everything was working as expected, and devices were receiving their IP addresses from the configured range.
```

This step-by-step document provides a practical guide to setting up DHCP on a Linux server, including all the basic configurations and troubleshooting tips.
