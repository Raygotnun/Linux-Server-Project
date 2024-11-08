This document for setting up a Kali Linux client workstation with **LibreOffice**, **Gimp**, and **Mullvad browser**. This document also covers setting up automatic IP addressing (using DHCP) and configuring the `/home` folder on a separate partition.

---

# **Linux Workstation Setup Guide**

This document provides a step-by-step guide for setting up a Kali Linux client workstation with the following applications:

- **LibreOffice**
- **Gimp**
- **Mullvad Browser**

It also ensures that the workstation:
- Uses **automatic IP addressing** (DHCP).
- Has the `/home` folder located on a **separate partition** on the same disk.

---

## **1. Prerequisites**

- Ensure you have **root access** on the Kali Linux machine.
- Confirm that you have an **internet connection** to download and install applications.
- Identify the **disk and partition** information if you're setting up a separate `/home` partition.

---

## **2. Setting Up Automatic IP Addressing (DHCP)**

### **Check Network Interface**

1. Open a terminal.
2. Identify your network interface name:
   ```bash
   ip link show
   ```

   Common interface names may be `eth0`, `wlan0`, or `enp0s3`.

### **Configure DHCP for Automatic Addressing**

1. Edit the **network configuration** file for DHCP:
   ```bash
   sudo nano /etc/network/interfaces
   ```

2. Add or modify the configuration to enable DHCP on your primary network interface. Replace `eth0` with your network interface name if it differs.
   ```plaintext
   auto eth0
   iface eth0 inet dhcp
   ```

3. **Save and close** the file.
4. Restart networking services:
   ```bash
   sudo systemctl restart networking
   ```

---

## **3. Set Up a Separate Partition for `/home`**

### **Partitioning Disk**

1. Open a terminal.
2. Use `fdisk` or `parted` to create a new partition for the `/home` directory on your existing disk (e.g., `/dev/sda`):
   ```bash
   sudo fdisk /dev/sda
   ```
3. Follow the prompts to create a new partition of the desired size (e.g., 10GB or 5GB as needed). Take note of the new partition name (e.g., `/dev/sda3`).

4. **Format** the new partition:
   ```bash
   sudo mkfs.ext4 /dev/sda3
   ```

### **Mount the New Partition to `/home`**

1. **Mount the partition temporarily** to copy existing data:
   ```bash
   sudo mount /dev/sda3 /mnt
   ```

2. **Copy the current `/home` data** to the new partition:
   ```bash
   sudo rsync -aXS /home/ /mnt/
   ```

3. **Unmount** the new partition:
   ```bash
   sudo umount /mnt
   ```

4. **Edit the `/etc/fstab`** file to mount the partition at `/home` on boot:
   ```bash
   sudo nano /etc/fstab
   ```
   Add the following line at the end of the file:
   ```plaintext
   /dev/sda3   /home   ext4   defaults   0   2
   ```

5. **Reboot the system** to apply changes:
   ```bash
   sudo reboot
   ```

---

## **4. Install Required Applications**

### **4.1 Installing LibreOffice**

1. Open a terminal and update the package list:
   ```bash
   sudo apt update
   ```
2. Install LibreOffice:
   ```bash
   sudo apt install libreoffice -y
   ```

### **4.2 Installing GIMP**

1. Install GIMP by running:
   ```bash
   sudo apt install gimp -y
   ```

### **4.3 Installing Mullvad Browser**

1. Download the Mullvad Browser from the official [Mullvad website](https://mullvad.net/en/download/browser/).
   ```bash
   wget -O mullvad-browser.tar.xz https://mullvad.net/media/mullvad-browser/linux-64/
   ```
2. **Extract** the archive:
   ```bash
   tar -xvf mullvad-browser.tar.xz
   ```
3. **Run** Mullvad Browser:
   ```bash
   ./mullvad-browser/mullvad-browser
   ```
4. For easy access, create a shortcut or launcher for Mullvad Browser.

---

## **5. Verify Configuration**

1. **Check DHCP Status**:
   ```bash
   ip a show <interface_name>
   ```
   Confirm the interface has obtained an IP address from the DHCP server.

2. **Confirm `/home` Partition**:
   ```bash
   df -h /home
   ```
   Ensure that `/home` is mounted on the new partition (e.g., `/dev/sda3`).

3. **Launch Installed Applications**:
   - Open LibreOffice, GIMP, and Mullvad Browser to ensure they are installed and functioning correctly.

---

## **6. Maintenance Tips**

- **Regularly update** the system and applications to get the latest security patches:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
- **Backup** the `/home` partition regularly to avoid data loss.

---

**End of Document**
