
# How I Set Up a DNS Server on a Linux Server Using BIND (Named)

Here’s the step-by-step process I followed to set up a DNS server on my Linux server using BIND (Berkeley Internet Name Domain) with **named** as the service.

---

## Step 1: System Preparation

First, I made sure my system was up to date to avoid any compatibility issues and ensure all software is up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Installing DNS Server Software

I used **BIND9** because it is a widely-used, flexible DNS server. The service name in modern Linux distributions using BIND9 is `named`. To install it, I ran:

```bash
sudo apt install bind9 bind9utils bind9-doc -y
```

- `bind9`: The BIND DNS server software.
- `bind9utils`: Utilities for managing and troubleshooting the DNS server.
- `bind9-doc`: Documentation for configuring and using BIND.

## Step 3: Configuring DNS Server

The main configuration file for BIND is `/etc/bind/named.conf`. There are several files involved in the configuration, but the main ones are:

- **/etc/bind/named.conf.options**: Configuration for general DNS server options.
- **/etc/bind/named.conf.local**: Contains the zone files for local DNS records.
- **/etc/bind/named.conf.default-zones**: Configuration for default zones, like the root DNS servers.

### Configuring Named Options

The first thing I did was configure the general settings for BIND by editing `/etc/bind/named.conf.options`. This file controls DNS server settings such as forwarding, recursion, and access control.

To open the file:

```bash
sudo nano /etc/bind/named.conf.options
```

Inside this file, I enabled forwarding to Google's public DNS servers and restricted recursion for internal clients:

```conf
options {
    directory "/var/cache/bind";

    // Forwarding to external DNS servers (e.g., Google DNS)
    forwarders {
        8.8.8.8;
        8.8.4.4;
    };

    // Allow recursion from the local network
    allow-recursion { 10.0.0.0/24; };  // Adjust to your network

    // Other options...
    listen-on { any; };
    listen-on-v6 { any; };
};
```

- **forwarders**: Specifies the DNS servers to which queries will be forwarded if they cannot be resolved locally.
- **allow-recursion**: Restricts recursion to clients on the local network (e.g., `10.0.0.0/24`).
- **listen-on**: Defines the interfaces on which the server listens for DNS queries.

### Configuring Zone Files

Next, I configured my DNS zones by editing `/etc/bind/named.conf.local`. Zone files contain the DNS records for domain names (like `library.local`).

To open the file:

```bash
sudo nano /etc/bind/named.conf.local
```

Inside this file, I added a zone configuration for `library.local`:

```conf
zone "library.local" {
    type master;
    file "/etc/bind/db.library.local";  // Path to the zone file
};
```

This configuration tells **named** to use the file `/etc/bind/db.library.local` for the zone `library.local`.

### Creating the Zone File

I created the zone file `/etc/bind/db.library.local` to define the DNS records for `library.local` (like `A`, `MX`, and `CNAME` records):

```bash
sudo nano /etc/bind/db.library.local
```

Here’s an example zone file with basic DNS records:

```conf
$TTL 86400
@    IN    SOA   ns1.library.local. admin.library.local. (
                 2024110601 ; Serial
                 3600       ; Refresh
                 1800       ; Retry
                 1209600    ; Expire
                 86400 )    ; Minimum TTL

     IN    NS    ns1.library.local.
     IN    NS    ns2.library.local.

ns1  IN    A     10.0.0.1
ns2  IN    A     10.0.0.2

www  IN    A     10.0.0.15
```

- **SOA Record**: Defines the Start of Authority for the domain, including the admin email and serial number.
- **NS Records**: Specifies the nameservers for the domain.
- **A Records**: Maps hostnames (like `ns1`, `www`) to IP addresses.

### Setting the Local DNS to Listen on the Correct Interface

Next, I made sure that **named** listens for DNS requests on the correct interface. This is done in `/etc/bind/named.conf.options`.

In the `options` block, I ensured that **named** was listening on the correct IP:

```conf
listen-on { 10.0.0.1; };
```

You can also use `listen-on-v6` for IPv6 configurations.

## Step 4: Restarting Named and Enabling the Service

Once the configuration was complete, I restarted the **named** service to apply the changes:

```bash
sudo systemctl restart named
```

I also enabled **named** to start on boot:

```bash
sudo systemctl enable named
```

## Step 5: Verifying DNS Server

To verify that **named** was running correctly, I checked the status of the service:

```bash
sudo systemctl status named
```

If it was running without errors, that meant the DNS server was working.

Next, I tested that DNS resolution was functioning by running a query with `dig`:

```bash
dig @10.0.0.1 library.local
```

This command should return the IP address for `library.local` if the DNS server is properly configured.

## Step 6: Troubleshooting (If Necessary)

If there were any issues, I checked the logs to find out what went wrong. The logs for **named** can be found in `/var/log/syslog`:

```bash
sudo tail -f /var/log/syslog
```

The logs should provide details if there are any errors in the configuration or if the DNS server isn't working correctly.

---

## Conclusion

By following these steps, I successfully set up a DNS server using **named** on a Linux machine. The server was able to resolve domain names for my internal network (`library.local`), and I could use the server to resolve domain names for external sites via forwarding.

The DNS server was tested using the `dig` command and confirmed to be working, with proper resolution for both internal and external queries.
