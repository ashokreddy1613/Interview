### In Linux, how do you attach and detach a filesystem?
In Linux, attaching and detaching a filesystem refers to mounting and unmounting a storage device (like a disk partition, USB drive, or network volume). 

### How do you print the last 15 lines of a file in Linux?
You can print the last 15 lines of a file in Linux using the tail command

### In Linux systems there is the term Load Average? what does that mean? how it is being calculated? and in what format the load average output is?
Load Average in Linux shows the average number of runnable processes over 1, 5, and 15 minutes. It's calculated using an exponentially weighted moving average, and helps evaluate whether the CPU is under stress, saturated, or idle.

### Write a shell script to find and delete all files in a directory that are older than 30 days.
### Create a script to monitor the disk usage of a server. If usage exceeds 80%, log the details to a file and send an alert email.
### Write a script that renames all .txt files in a directory by appending the current date to the filename.-not Answered

### Cron expression to schedule a job in Linux
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
### what contains inside vat and opt in linux.

1. /var – Variable Data Directory
/var stands for “variable” and contains data that changes frequently during normal system operation.
```bash
/var/log/	System and application log files
/var/spool/	Queued tasks: print jobs, cron jobs, mail
/var/lib/	Dynamic state info (e.g., package dbs, databases)
/var/cache/	Temporary cached data
/var/tmp/	Temporary files preserved between reboots
```
2. /opt – Optional Software Directory
/opt stands for “optional” and is used for third-party software that’s not part of the default Linux distribution.
```bash
opt/myapp/	A custom or third-party application
/opt/google/chrome/	Chrome installed from a .deb/.rpm file
/opt/<vendor>/<app>	Vendor-specific software
```
### Diff between mount and directories in Linux
A directory is just a folder, part of your existing filesystem.
A mount is the process of attaching a separate filesystem to that directory, making its contents accessible there.

### How will you restart http service from VM
To restart the HTTP service on a Linux VM, I typically use sudo systemctl restart httpd (for Apache) or apache2 (on Ubuntu), depending on the OS.

## Disk I/O
Disk I/O (Input/Output) refers to the read and write operations that occur between the system and the storage device (e.g., hard disk, SSD, or block storage like EBS).
n one case, a PostgreSQL database was slow due to EBS burst credits being exhausted.
iostat showed 100% disk utilization and high await times — we moved to a higher throughput volume (gp3), and performance improved instantly.

###What is meant by CPU throttling
CPU throttling refers to the process of intentionally limiting the CPU usage of a process or system

CPU throttling is the act of limiting CPU speed or usage — either to protect hardware (thermal) or to enforce resource limits (like in containers or VMs).
While it helps with control and stability, excessive throttling can cause application performance issues.

Write a shell script that checks if a service is running, restarts it if not, and logs the event.
### Any experience on phython/shell scripting? can you explain one file?-notAnswered
### How to check the linux process
### Booting in Linux
###How to check load of linux machine
While rebooting a Linux machine, what are the stages / layers will be restarted
Kernel logs are stored under which directory 
How to kill the running process
States of Linux machine
When you type google.com, what will happen at backend in browser 
When you type TOP command, what are the components will be displayed
Write a script to monitor a directory and automatically copy any new files to a remote server using SCP.
Write a shell script where you have one virtual machine ubuntu1, auto ssh enabled, ssh -i for private key, directory path /nobackup to be copied in another VM. 
 Jenkins p

 Can we create AWS backup using Shell Scripting?
Once the backup is created, where will you store the log files?
explain did u done any automation in your project 