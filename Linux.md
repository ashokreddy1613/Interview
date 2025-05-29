# Linux Interview Questions and Answers

## Q1: In Linux, how do you attach and detach a filesystem?
In Linux, attaching and detaching a filesystem refers to mounting and unmounting a storage device (like a disk partition, USB drive, or network volume). 

## Q2: How do you print the last 15 lines of a file in Linux?
You can print the last 15 lines of a file in Linux using the tail command.

## Q3: In Linux systems there is the term Load Average? What does that mean? How it is being calculated? And in what format the load average output is?
Load Average in Linux shows the average number of runnable processes over 1, 5, and 15 minutes. It's calculated using an exponentially weighted moving average, and helps evaluate whether the CPU is under stress, saturated, or idle.

## Q4: Write a shell script to find and delete all files in a directory that are older than 30 days.

## Q5: Create a script to monitor the disk usage of a server. If usage exceeds 80%, log the details to a file and send an alert email.

## Q6: Write a script that renames all .txt files in a directory by appending the current date to the filename.

## Q7: What is the cron expression to schedule a job in Linux?
In Linux, a cron expression is a 5-field time format used in crontab to schedule recurring jobs.
Each field defines when a command should run: minute, hour, day of month, month, and day of week.
```bash
* * * * * <command>
│ │ │ │ │
│ │ │ │ └─ Day of week (0–7) → Sun/Sat (0 or 7)
│ │ │ └─── Month (1–12)
│ │ └───── Day of month (1–31)
│ └─────── Hour (0–23)
└───────── Minute (0–59)
```

## Q8: What contains inside /var and /opt in Linux?

1. /var – Variable Data Directory
/var stands for "variable" and contains data that changes frequently during normal system operation.
```bash
/var/log/	System and application log files
/var/spool/	Queued tasks: print jobs, cron jobs, mail
/var/lib/	Dynamic state info (e.g., package dbs, databases)
/var/cache/	Temporary cached data
/var/tmp/	Temporary files preserved between reboots
```

2. /opt – Optional Software Directory
/opt stands for "optional" and is used for third-party software that's not part of the default Linux distribution.
```bash
opt/myapp/	A custom or third-party application
/opt/google/chrome/	Chrome installed from a .deb/.rpm file
/opt/<vendor>/<app>	Vendor-specific software
```

## Q9: What is the difference between mount and directories in Linux?
A directory is just a folder, part of your existing filesystem.
A mount is the process of attaching a separate filesystem to that directory, making its contents accessible there.

## Q10: How will you restart HTTP service from VM?
To restart the HTTP service on a Linux VM, I typically use sudo systemctl restart httpd (for Apache) or apache2 (on Ubuntu), depending on the OS.

## Q11: What is Disk I/O?
Disk I/O (Input/Output) refers to the read and write operations that occur between the system and the storage device (e.g., hard disk, SSD, or block storage like EBS).
In one case, a PostgreSQL database was slow due to EBS burst credits being exhausted.
iostat showed 100% disk utilization and high await times — we moved to a higher throughput volume (gp3), and performance improved instantly.

## Q12: What is meant by CPU throttling?
CPU throttling refers to the process of intentionally limiting the CPU usage of a process or system.
CPU throttling is the act of limiting CPU speed or usage — either to protect hardware (thermal) or to enforce resource limits (like in containers or VMs).
While it helps with control and stability, excessive throttling can cause application performance issues.

## Q13: Write a shell script that checks if a service is running, restarts it if not, and logs the event.
```bash
#!/bin/bash
SERVICE_NAME="apache2"
LOG_FILE="/var/log/service_monitor.log"

# Check if service is running
if systemctl is-active --quiet $SERVICE_NAME; then
    echo "$(date): $SERVICE_NAME is running" >> $LOG_FILE
else
    echo "$(date): $SERVICE_NAME is not running. Attempting to restart..." >> $LOG_FILE
    systemctl restart $SERVICE_NAME
    
    # Verify if restart was successful
    if systemctl is-active --quiet $SERVICE_NAME; then
        echo "$(date): $SERVICE_NAME successfully restarted" >> $LOG_FILE
    else
        echo "$(date): Failed to restart $SERVICE_NAME" >> $LOG_FILE
    fi
fi
```

## Q14: Any experience on Python/shell scripting? Can you explain one file?
Yes, I have experience with both Python and shell scripting. Here's an example of a Python script that monitors system resources:

```python
#!/usr/bin/env python3
import psutil
import time
import logging

logging.basicConfig(filename='system_monitor.log', level=logging.INFO)

def monitor_resources():
    while True:
        cpu_percent = psutil.cpu_percent()
        memory = psutil.virtual_memory()
        disk = psutil.disk_usage('/')
        
        logging.info(f"CPU Usage: {cpu_percent}%")
        logging.info(f"Memory Usage: {memory.percent}%")
        logging.info(f"Disk Usage: {disk.percent}%")
        
        time.sleep(300)  # Check every 5 minutes

if __name__ == "__main__":
    monitor_resources()
```

## Q15: How to check the Linux process?
There are several commands to check Linux processes:
1. `ps` - Display current processes
   ```bash
   ps aux  # Show all processes for all users
   ps -ef  # Show all processes in full format
   ```
2. `top` - Interactive process viewer
3. `htop` - Enhanced version of top
4. `pgrep` - Find processes by name
   ```bash
   pgrep apache2  # Find Apache processes
   ```
5. `pidof` - Find process ID by name
   ```bash
   pidof nginx  # Get Nginx process ID
   ```

## Q16: What is the booting process in Linux?
The Linux booting process consists of several stages:
1. BIOS/UEFI - Performs hardware checks and loads the bootloader
2. Bootloader (GRUB) - Loads the Linux kernel
3. Kernel Initialization - Mounts root filesystem
4. Systemd (or init) - First process (PID 1)
5. Runlevels/Targets - System initialization
6. Login Prompt - User authentication

## Q17: How to check load of Linux machine?
You can check system load using several methods:
1. `uptime` command
   ```bash
   uptime  # Shows load averages for 1, 5, and 15 minutes
   ```
2. `top` command
3. `htop` command
4. `vmstat` command
   ```bash
   vmstat 1  # Update every second
   ```
5. `/proc/loadavg` file
   ```bash
   cat /proc/loadavg
   ```

## Q18: While rebooting a Linux machine, what are the stages/layers that will be restarted?
The reboot process follows these stages:
1. System shutdown sequence
   - Stop services gracefully
   - Unmount filesystems
   - Sync data to disk
2. Hardware reset
3. BIOS/UEFI initialization
4. Bootloader execution
5. Kernel loading
6. Systemd initialization
7. Service startup
8. Network initialization
9. User login prompt

## Q19: Where are kernel logs stored?
Kernel logs are primarily stored in:
1. `/var/log/kern.log` - Main kernel log file
2. `/var/log/syslog` - System logs including kernel messages
3. `dmesg` command output
4. `/var/log/dmesg` - Boot-time kernel messages
5. `journalctl` - Systemd journal logs
   ```bash
   journalctl -k  # View kernel logs
   ```

## Q20: How to kill the running process?
There are several ways to kill processes:
1. `kill` command
   ```bash
   kill <PID>  # Send SIGTERM (15)
   kill -9 <PID>  # Send SIGKILL (9)
   ```
2. `pkill` command
   ```bash
   pkill process_name
   ```
3. `killall` command
   ```bash
   killall process_name
   ```
4. `systemctl` for services
   ```bash
   systemctl stop service_name
   ```

## Q21: What are the states of Linux machine?
Linux processes can be in several states:
1. Running (R)
2. Sleeping (S)
3. Uninterruptible Sleep (D)
4. Stopped (T)
5. Zombie (Z)
6. Dead (X)

## Q22: When you type google.com, what will happen at backend in browser?
When you type google.com in a browser:
1. DNS Resolution
   - Browser checks local DNS cache
   - Queries DNS servers
   - Resolves domain to IP address
2. TCP Connection
   - Establishes TCP connection to server
   - Performs TCP handshake
3. TLS/SSL Handshake
   - Negotiates encryption
   - Verifies certificates
4. HTTP Request
   - Sends GET request
   - Includes headers and cookies
5. Server Processing
   - Server processes request
   - Generates response
6. Response
   - Browser receives response
   - Renders page content

## Q23: When you type TOP command, what are the components that will be displayed?
The `top` command displays:
1. System summary
   - Uptime
   - Load averages
   - Total tasks
   - CPU usage
   - Memory usage
2. Process list
   - PID
   - User
   - Priority
   - CPU usage
   - Memory usage
   - Time
   - Command

## Q24: Write a script to monitor a directory and automatically copy any new files to a remote server using SCP.
```bash
#!/bin/bash
WATCH_DIR="/path/to/watch"
REMOTE_USER="user"
REMOTE_HOST="remote.server.com"
REMOTE_DIR="/path/on/remote"
LOG_FILE="/var/log/file_sync.log"

# Monitor directory for new files
inotifywait -m -r -e create,moved_to "$WATCH_DIR" | while read path action file; do
    echo "$(date): New file detected: $file" >> $LOG_FILE
    
    # Copy file to remote server
    scp "$path$file" "$REMOTE_USER@$REMOTE_HOST:$REMOTE_DIR"
    
    if [ $? -eq 0 ]; then
        echo "$(date): Successfully copied $file" >> $LOG_FILE
    else
        echo "$(date): Failed to copy $file" >> $LOG_FILE
    fi
done
```

## Q25: Write a shell script where you have one virtual machine ubuntu1, auto ssh enabled, ssh -i for private key, directory path /nobackup to be copied in another VM.
```bash
#!/bin/bash
SOURCE_VM="ubuntu1"
SOURCE_DIR="/nobackup"
DEST_VM="destination-vm"
DEST_DIR="/backup"
SSH_KEY="/path/to/private_key"
LOG_FILE="/var/log/backup.log"

# Create backup with timestamp
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_$TIMESTAMP.tar.gz"

# Compress source directory
ssh -i $SSH_KEY $SOURCE_VM "tar -czf - $SOURCE_DIR" > $BACKUP_NAME

# Copy to destination VM
scp -i $SSH_KEY $BACKUP_NAME $DEST_VM:$DEST_DIR

# Verify transfer
if [ $? -eq 0 ]; then
    echo "$(date): Backup completed successfully" >> $LOG_FILE
    rm $BACKUP_NAME  # Clean up local backup
else
    echo "$(date): Backup failed" >> $LOG_FILE
fi
```

## Q26: Can we create AWS backup using Shell Scripting?
Yes, AWS backups can be created using shell scripting with AWS CLI. Here's an example:
```bash
#!/bin/bash
INSTANCE_ID="i-1234567890abcdef0"
BACKUP_TAG="Name=Backup,Value=Daily"
VOLUME_ID="vol-1234567890abcdef0"

# Create snapshot
aws ec2 create-snapshot \
    --volume-id $VOLUME_ID \
    --description "Daily backup $(date +%Y-%m-%d)" \
    --tag-specifications "ResourceType=snapshot,Tags=[$BACKUP_TAG]"

# Verify snapshot creation
if [ $? -eq 0 ]; then
    echo "Backup created successfully"
else
    echo "Backup failed"
fi
```

## Q27: Once the backup is created, where will you store the log files?
Log files for backups should be stored in a structured location:
1. `/var/log/backups/` - Main backup log directory
2. `/var/log/aws/` - AWS-specific logs
3. CloudWatch Logs - For AWS services
4. S3 bucket - For long-term log storage
5. Centralized logging system (e.g., ELK Stack)

## Q28: Explain did you done any automation in your project?
Yes, I have implemented several automation solutions:
1. Infrastructure Automation
   - Terraform for infrastructure provisioning
   - Ansible for configuration management
   - Jenkins for CI/CD pipelines

2. Monitoring Automation
   - Custom scripts for resource monitoring
   - Automated alerting system
   - Log aggregation and analysis

3. Backup Automation
   - Automated database backups
   - File system backups
   - Disaster recovery testing

4. Security Automation
   - Automated security scanning
   - Compliance checking
   - Access management

5. Deployment Automation
   - Automated testing
   - Zero-downtime deployments
   - Rollback procedures 