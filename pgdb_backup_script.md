# PostgreSQL Database Backup Automation

## Overview
This script automates PostgreSQL database backups with compression, retention management, and Telegram notifications.

## Features
- Automated backups of all PostgreSQL databases
- Backup compression using gzip
- Configurable retention policy
- Telegram notifications for success/failure
- Detailed logging

## Requirements

- Python 3 (`/usr/bin/python3`)

## Configuration

###  Script Location
1. Create script directory:
```bash
mkdir /crystal/
cd /crystal/
```
2. Create backup script:
```bash
vim pgdb_backup.py

```

```python
import os
import subprocess
import datetime
from pathlib import Path
import logging
import gzip
import shutil
import requests

# Configuration
BASE_BACKUP_DIR = "/var/backups/pgdb"   # Base directory for backups
TIMESTAMP = datetime.datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
BACKUP_DIR = f"{BASE_BACKUP_DIR}/db_backup-{TIMESTAMP}"
LOG_FILE = "/var/log/pgdb_backup.log"
RETENTION_DAYS = 7                   # Number of days to keep backups

DB_USER = "postgres"                 # PostgreSQL username
DB_PASSWORD = "postgrespassword"     # PostgreSQL password
DB_HOST = "localhost"                # Database host

TELEGRAM_BOT_TOKEN = "bot_token"     # Get this from @BotFather after creating a new bot
TELEGRAM_CHAT_ID = "chat_id"         # Your user or group chat ID

# Ensure backup directory exists
Path(BACKUP_DIR).mkdir(parents=True, exist_ok=True)

# Configure logging
logging.basicConfig(filename=LOG_FILE, level=logging.INFO, format='%(message)s')
logging.info("----------------------------------------------------")
logging.info(f"Backup started at: {datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
logging.info(f"Backup directory: {BACKUP_DIR}")
logging.info("----------------------------------------------------")

# Telegram alert function
def send_telegram_message(message: str):
    url = f"https://api.telegram.org/bot{TELEGRAM_BOT_TOKEN}/sendMessage"
    data = {
        "chat_id": TELEGRAM_CHAT_ID,
        "text": message,
        "parse_mode": "HTML"
    }
    try:
        response = requests.post(url, data=data, timeout=10)
        if not response.ok:
            logging.error(f"Failed to send Telegram message: {response.text}")
    except Exception as e:
        logging.error(f"Exception sending Telegram message: {str(e)}")

# Set environment variable for password
os.environ["PGPASSWORD"] = DB_PASSWORD

# Get list of databases (exclude templates)
try:
    result = subprocess.run(
        ["psql", "-U", DB_USER, "-h", DB_HOST, "-t", "-c",
         "SELECT datname FROM pg_database WHERE datistemplate = false;"],
        check=True, capture_output=True, text=True
    )
    databases = [db.strip() for db in result.stdout.strip().split('\n') if db.strip()]
except subprocess.CalledProcessError as e:
    error_msg = f"Failed to get database list:\n{e.stderr}"
    logging.error(error_msg)
    send_telegram_message(f"<b>Postgres Backup Error</b>:\n{error_msg}")
    exit(1)

# Backup each database into BACKUP_DIR, compress each
logging.info("Starting database backups...")
for db in databases:
    backup_filename = f"{db}_{TIMESTAMP}.backup"
    backup_path = os.path.join(BACKUP_DIR, backup_filename)
    logging.info(f"Backing up database: {db} to {backup_path}")
    try:
        # Run pg_dump for each DB
        subprocess.run(
            ["pg_dump", "-U", DB_USER, "-h", DB_HOST, "-F", "c", "-b", "-v", "-f", backup_path, db],
            check=True, capture_output=True, text=True
        )
        logging.info(f"Backup completed for database: {db}")
    except subprocess.CalledProcessError as e:
        error_msg = f"Backup failed for database: {db}\n{e.stderr}"
        logging.error(error_msg)
        send_telegram_message(f"<b>Backup failed for DB:</b> {db}\n<pre>{e.stderr}</pre>")
        continue

    # Compress the backup file
    try:
        with open(backup_path, 'rb') as f_in, gzip.open(f"{backup_path}.gz", 'wb') as f_out:
            shutil.copyfileobj(f_in, f_out)
        os.remove(backup_path)  # Remove uncompressed backup
        logging.info(f"Compressed backup created: {backup_path}.gz")
    except Exception as e:
        error_msg = f"Compression failed for {backup_path}\n{str(e)}"
        logging.error(error_msg)
        send_telegram_message(f"<b>Compression failed for:</b> {backup_path}\n<pre>{str(e)}</pre>")

logging.info("----------------------------------------------------")

# Delete old backup folders older than RETENTION_DAYS
logging.info(f"Removing backup folders older than {RETENTION_DAYS} days...")
now = datetime.datetime.now()
for folder in Path(BASE_BACKUP_DIR).iterdir():
    if folder.is_dir() and folder.name.startswith("db_backup-"):
        try:
            folder_time = datetime.datetime.strptime(folder.name.replace("db_backup-", ""), "%Y-%m-%d_%H-%M-%S")
            if folder_time < (now - datetime.timedelta(days=RETENTION_DAYS)):
                shutil.rmtree(folder)
                logging.info(f"Deleted old backup folder: {folder}")
        except ValueError:
            # Folder name didn't match expected pattern; skip
            continue

# Cleanup environment variable
os.environ.pop("PGPASSWORD", None)

logging.info("===================================================================")
logging.info(f"Backup completed at: {datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
logging.info("===================================================================")
logging.info("\n\n\n")

```
3. Make script executable:

```bash
chmod +x pgdb_backup.py
```

> ⚠️ **Keep sensitive data secure. Avoid hardcoding passwords and tokens in production.**
--- 

## 🗓️ Cronjob Setup (Daily at 10:00 PM)

### Edit crontab:
```bash
crontab -e
```

### Add line:
```bash
0 22 * * * /usr/bin/python3 /crystal/pgdb_backup.py >> /var/log/cron_pg_backup.log 2>&1
```

#### Meaning:
- `0 22 * * *`: At 10:00 PM every day
- `>> /var/log/cron_pg_backup.log 2>&1`: Append output and error to cron log

---

## 📂 Backup Output

- Location: `/var/backups/pgdb/db_backup-YYYY-MM-DD_HH-MM-SS/`
- File format: `<dbname>_YYYY-MM-DD_HH-MM-SS.backup.gz`

### Backup Structure
Backups are stored with timestamped folders:

```
/var/backups/pgdb/
└── db_backup-2023-12-31_22-00-00/
    ├── database1_2023-12-31_22-00-00.backup.gz
    └── database2_2023-12-31_22-00-00.backup.gz
```

## Logging
Detailed logs are maintained at:
- /var/log/pgdb_backup.log (`script logs`)
- /var/log/cron_pg_backup.log (`cron job logs`)
---


## Telegram Notification

### Error Handling
The script provides detailed error notifications via Telegram, including:
- Connection failures
- Backup failures for individual databases
- Compression issues
- Retention cleanup problems

Example error notification:

```
Postgres Backup Error:
Failed to get database list:
psql: error: connection to server at "localhost" (::1), port 5432 failed: FATAL:  password authentication failed for user "postgres"
connection to server at "localhost" (::1), port 5432 failed: FATAL:  password authentication failed for user "postgres"
```