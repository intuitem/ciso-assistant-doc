# Automating Database Backups in CISO Assistant
This guide explains how to automate the backup of the CISO Assistant database on a VPS, ensuring that:
- Backups are created weekly.
- Each backup is named with a timestamp (YYYYMMDD).
- Only the last two backups are retained.
- All backup operations are logged for auditing purposes.

## Prerequisites
- A Linux-based VPS hosting the CISO Assistant instance.
- Basic knowledge of terminal commands.
- Access to the system’s crontab for scheduling tasks.

## Step 1: Create the Backup Script
To automate the backup process, create a script that will:

- Copy the CISO Assistant database file.
- Store the backup with a timestamp.
- Maintain only the two most recent backups.
- Log all backup activities.

### 1.1 Create the Script File
Run the following command to create a new script file:
```sh
nano /home/user/ciso-backup.sh
```
Replace `/home/user/` with your actual user directory.

### 1.2 Add the Following Script Content
Copy and paste the script below into the file:
```sh
#!/bin/bash

# Configuration
BACKUP_DIR="/home/user/backups"
LOG_FILE="/home/user/ciso-backup.log"
DB_PATH="/path/to/ciso-assistant-community/db/ciso-assistant.sqlite3"

# Ensure the backup directory exists
mkdir -p "$BACKUP_DIR"

# Define the backup filename with a timestamp
BACKUP_FILE="ciso-assistant-backup-$(date +%Y%m%d).sqlite3"

# Start logging
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Starting backup..." >> "$LOG_FILE"

# Check if the database file exists before proceeding
if [ -f "$DB_PATH" ]; then
    cp "$DB_PATH" "$BACKUP_DIR/$BACKUP_FILE"

    if [ $? -eq 0 ]; then
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] Backup successful: $BACKUP_FILE" >> "$LOG_FILE"
    else
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] ERROR: Backup failed!" >> "$LOG_FILE"
        exit 1
    fi
else
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] ERROR: Database file not found at $DB_PATH" >> "$LOG_FILE"
    exit 1
fi

# Retain only the last two backups and delete older ones
ls -tp "$BACKUP_DIR"/ciso-assistant-backup-*.sqlite3 | grep -v '/$' | tail -n +3 | xargs -d '\n' rm -f

# Log cleanup process
if [ $? -eq 0 ]; then
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] Old backups cleaned up successfully." >> "$LOG_FILE"
else
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] ERROR: Failed to clean up old backups!" >> "$LOG_FILE"
fi

echo "[$(date '+%Y-%m-%d %H:%M:%S')] Backup process completed." >> "$LOG_FILE"
```
Note: Replace `/path/to/ciso-assistant-community/db/` with the actual path to your SQLite database file.

### 1.3 Save and Exit
Press `CTRL+O`, then `CTRL+X`, then `Y`, and hit Enter to save the script.

### 1.4 Make the Script Executable
Run the following command to give execution permissions:
```sh
chmod +x /home/user/ciso-backup.sh
```

## Step 2: Schedule Automatic Backups with Cron
To automate the backup process, we will schedule a cron job to run the script weekly.

### 2.1 Open the Crontab
Run:
```sh
crontab -e
```
This will open the crontab editor.

### 2.2 Add the Cron Job
Add the following line at the end of the file:
```sh
0 3 * * 0 /home/user/ciso-backup.sh >> /home/user/ciso-cron.log 2>&1
```
### Explanation:
`0 3 * * 0` → Runs every Sunday at 3:00 AM.
`/home/user/ciso-backup.sh` → Executes the backup script.
`>> /home/user/ciso-cron.log 2>&1` → Logs cron job output to /home/user/ciso-cron.log.

### 2.3 Save and Exit
Press `CTRL+O`, then `CTRL+X`, then `Y`, and hit Enter to save the crontab.

### 2.4 Verify the Scheduled Cron Job
To confirm the cron job is set, run:
```sh
crontab -l
```
This should display the scheduled job.

## Step 3: Verify Backup and Logs
Once the cron job is running, you can verify backup success by checking the backup directory and logs.

### 3.1 Check Backups
Run:
```sh
ls -lh /home/user/backups/
```
You should see `.sqlite3` backup files named with their timestamps.

### 3.2 Check Backup Logs
To view backup logs:
```sh
cat /home/user/ciso-backup.log
```

Example log output:
```yaml
[2025-03-17 03:00:01] Starting backup...
[2025-03-17 03:00:02] Backup successful: ciso-assistant-backup-20250317.sqlite3
[2025-03-17 03:00:03] Old backups cleaned up successfully.
[2025-03-17 03:00:03] Backup process completed.
```

### 3.3 Check Cron Execution Logs
To view cron execution logs:
```sh
cat /home/user/ciso-cron.log
```

## Summary
- Automated Weekly Backups – Scheduled via cron.
- Timestamped Backup Files – Uses YYYYMMDD format.
- Retention Policy – Keeps only the last two backups.
- Logging Enabled – Tracks backup success and failure.

This setup ensures the CISO Assistant database is regularly backed up and protected against data loss.
