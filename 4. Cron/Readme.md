# Setting up a weekly backup system using Cron

---
## **Overview**

Cron is a built-in Linux utility that schedules and automates repetitive tasks by executing scripts or commands at specified times or dates. For backups, cron can run backup scripts that utilize tools like `tar`, `rsync`, or `cp` to copy files and directories to a secure location, such as an external drive or remote server. By adding a backup script to the crontab (cron table), you can schedule backups to occur automatically at intervals like daily, weekly, or monthly. This automation ensures that data is regularly saved without manual intervention, enhancing data security and recovery capabilities. Setting up cron-based backups is a fundamental practice for system maintenance and data protection on Linux systems.

---

## **Prerequisites**

- **Root or Sudo Access**: Necessary for installing packages and accessing system directories.
- **Backup Destination**: A local directory, external drive, or remote server where backups will be stored.
- **Essential Tools**: Ensure `rsync`, `tar`, and `gzip` are installed.

---

## **Step 1: Identify Data to Back Up**

Determine the files and directories that need to be backed up.
In our case

- **System Configurations**: `/etc/`
- **User Data**: `/root/`
- **Web Server Files**: `/var/www/` (if applicable)


---

## **Step 2: Install Necessary Tools**

Install `rsync`, `tar`, and `gzip` if they are not already installed:

```bash
sudo apt update
sudo apt install rsync tar gzip -y
```

---

## **Step 3: Create a Backup Script**

Create a backup script that will perform the backup tasks.

**Create the Script File:**

```bash
sudo nano /usr/local/bin/backup.sh
```

**Add the Following Script:**

```bash
#!/bin/bash

# Directories to back up
SOURCE_DIRS="/etc /root /var/www"

# Backup destination
DEST_DIR="/backup"

# Ensure the backup directory exists
mkdir -p "$DEST_DIR"

# Date format for backup filenames
DATE=$(date +"%Y-%m-%d_%H-%M-%S")

# Backup filename
BACKUP_FILE="$DEST_DIR/backup_$DATE.tar.gz"

# Perform the backup using tar
tar -czpf "$BACKUP_FILE" $SOURCE_DIRS

# Optional: Remove backups older than 7 days
find "$DEST_DIR" -type f -name "*.tar.gz" -mtime +7 -exec rm {} \;

# Log the backup
echo "[$(date +"%Y-%m-%d %H:%M:%S")] Backup created: $BACKUP_FILE" >> /var/log/backup.log
```

**Explanation of the Script:**

- **Variables**: Define source directories and backup destination.
- **Date Formatting**: Creates a unique filename based on the current date and time.
- **Backup Command**: Uses `tar` to create a compressed archive of the specified directories.
- **Cleanup**: Removes backups older than 7 days to save space.
- **Logging**: Appends a log entry to `/var/log/backup.log`.

---

## **Step 4: Make the Script Executable**

Set the execute permission on the backup script:

```bash
sudo chmod +x /usr/local/bin/backup.sh
```

---

## **Step 5: Test the Backup Script**

Run the script manually to ensure it works correctly:

```bash
sudo /usr/local/bin/backup.sh
```

**Verify the Backup:**

- Check that the backup file exists in `/backup`.
- Ensure the log entry has been added to `/var/log/backup.log`.
- Test extracting the backup to verify its integrity (optional).

---

## **Step 6: Schedule the Backup with CRON**

Use CRON to schedule the backup script to run automatically.

**Edit the Root User's CRON Table:**

```bash
sudo crontab -e
```

**Add the Following Line to Schedule the Backup at 0 AM Every Week:**

```bash
0 0 * * 0 /usr/local/bin/backup.sh
```

**CRON Schedule Format:**

- **Minute**: 0
- **Hour**: 2 (2 AM)
- **Day of Month**: *
- **Month**: *
- **Day of Week**: *

---

## **Step 7: Confirm the CRON Job**

List the CRON jobs to confirm the entry:

```bash
sudo crontab -l
```

You should see the line you added:

```bash
0 0 * * 0 /usr/local/bin/backup.sh
```

---

## **Step 8: Secure the Backup Files**

Ensure that the backup files are secure:

- **Permissions**: Restrict access to the backup directory.

```bash
sudo chmod -R 700 /backup
```

- **Ownership**: Ensure the backups are owned by root.

```bash
sudo chown -R root:root /backup
```

---

