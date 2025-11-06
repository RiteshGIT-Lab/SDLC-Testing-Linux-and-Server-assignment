# SDLC-Testing-Linux-and-Server-assignment
## HeroVired Linux assignment
## Task 1: System Monitoring Setup
## Task 1 Summary:
Install and configure htop or nmon to monitor CPU, memory, and processes, using df and du for disk usage tracking, and identifying resource-intensive processes. Proper logging of system metrics and clear documentation of the setup are essential. 

## Task 1: System Monitoring Setup Lab resluts:

 ## system monitoring using htop

installing htop: sudo update -y sudo install epel-release -y sudo install htop -y

after installing htop run command htop and i got following output

![Task 1 System Monitoring Setup snapshot](https://github.com/user-attachments/assets/693ee811-3f6e-4d7c-ad25-b2b16d9100b2)


we can monitor cpu, memory, processes etc by issuing command htop and then pressing f6 and than selecting metric from the list as given in snapshot below

![Task 1 System Monitoring Setup snapshot2](https://github.com/user-attachments/assets/833a9d8c-7843-4137-8c1e-070e8e0d96fc)

## system monitoring using nmon

installation steps: sudo update -y sudo install epel-release -y sudo install nmon -y

after installing htop run command nmon and i got following output

![Task 1 System Monitoring Setup snapshot3](https://github.com/user-attachments/assets/b1a26d66-12fd-49c0-b036-47d8317ddcf3)

as nmon is also interactive process so to get status of memory,cpu and processe following needs to be done:

issue command nmon in terminal than

for cpu : press 'c' to view cpu status
for memory : press 'm' to view memory status
for processes : press 'u' to view top memory and cpu consuming processes
for cpu follwing output observed:

![Task 1 System Monitoring Setup snapshot4](https://github.com/user-attachments/assets/510a549d-78f8-4270-9fc9-9a6f0a185999)

for memory following output observed:

![Task 1 System Monitoring Setup snapshot5](https://github.com/user-attachments/assets/8f355729-29bd-4e2c-b741-9acb6c71e966)

for top processes follwing output observed:
![Task 1 System Monitoring Setup snapshot6](https://github.com/user-attachments/assets/bfd04bc8-2dca-47f0-b607-f2f14d3a2ac9)


##Disk Usage Monitoring : follwing command is used for disk usage monitoring df -h

output

![Task 1 System Monitoring Setup snapshot7](https://github.com/user-attachments/assets/eed26086-8b3c-4609-b124-8895e790b3da)

to make a log file df -h > /var/log/disk_usage.log

output of a log file:

![Task 1 System Monitoring Setup snapshot8](https://github.com/user-attachments/assets/7b267ed1-5547-4297-a3f5-d79f4a36af49)

To automate daily at 8 am in morning: echo "0 8 * * * df -h > /var/log/disk_usage.log" | sudo tee -a /etc/crontab

Identifying Resource-Intensive Processes and logging to log file

ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 10 > /var/log/top_processes.log

output of a log file:

![Task 1 System Monitoring Setup snapshot9](https://github.com/user-attachments/assets/9b0e99bc-eb12-4e66-8082-1f766bb5e252)


to automate on a hourly basis:

echo "0 * * * * ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 10 > /var/log/top_processes.log" | sudo tee -a /etc/crontab

making a script for monitoring purpose: Create a script /usr/local/bin/system_monitor.sh:

#!/bin/bash LOG_FILE="/var/log/system_monitor_$(date +%F).log"

echo "===== System Report $(date) =====" >> $LOG_FILE echo "--- CPU/Memory Usage ---" >> $LOG_FILE top -b -n1 | head -n 10 >> $LOG_FILE

echo "--- Disk Usage ---" >> $LOG_FILE df -h >> $LOG_FILE

echo "--- Top Processes ---" >> $LOG_FILE ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 10 >> $LOG_FILE

echo "===================================" >> $LOG_FILE

changing permission of above script file: sudo chmod +x /usr/local/bin/system_monitor.sh

running it hourly basis: echo "0 * * * * root /usr/local/bin/system_monitor.sh" | sudo tee -a /etc/crontab

output of above script file generated in /var/log as system_monitor_current_date.log file

![Task 1 System Monitoring Setup completed snapshot](https://github.com/user-attachments/assets/73cf7f13-f6c0-4b32-a9d3-e88494d50dfe)

## Task 2: User Management and Access Control

## Task 2: User Management and Access Control summary
Evaluation includes creating user accounts for Sarah and Mike with secure passwords, setting up isolated directories with appropriate permissions, and enforcing a password policy with expiration and complexity. Detailed documentation of user management steps is required. 

## Step 1: Create User Accounts

Create users with home directories
sudo useradd -m -d /home/Sarah -s /bin/bash Sarah sudo useradd -m -d /home/mike -s /bin/bash mike

Set passwords for each user (you will be prompted to enter them)
sudo passwd Sarah sudo passwd mike

![Task 2 User Management and Access Control snapshot1](https://github.com/user-attachments/assets/a4acf3e2-f01b-4592-9711-e319316616ad)

## Step 2: Create Isolated Working Directories

Create dedicated workspace directories
sudo mkdir -p /home/Sarah/workspace sudo mkdir -p /home/mike/workspace

Set ownership and permissions
sudo chown Sarah:Sarah /home/Sarah/workspace sudo chown mike:mike /home/mike/workspace

Restrict access so only the owner can access their workspace
sudo chmod 700 /home/Sarah/workspace sudo chmod 700 /home/mike/workspace

output: /home/Sarah/workspace → accessible only by Sarah /home/mike/workspace → accessible only by Mike

![Task 2 User Management and Access Control snapshot2](https://github.com/user-attachments/assets/a3bf74cf-1f9d-4fa2-9f50-94c00b13df7e)

## Step 3: Configure Password Policy: sudo vi /etc/login.defs

PASS_MAX_DAYS 30 # Password expires every 30 days PASS_MIN_DAYS 1 # Minimum 1 day before password can be changed PASS_WARN_AGE 7 # Warn user 7 days before password expires

![Task 2 User Management and Access Control snapshot3](https://github.com/user-attachments/assets/9e1264ea-4050-4dbd-89fa-56f503fa02d3)


## Task 3 solution: Script for Sarah (Apache) 

## Task 3 solution: Script for Sarah (Apache) Summary
Configure automated backups for Apache (/etc/httpd/, /var/www/html/) and Nginx (/etc/nginx/, /usr/share/nginx/html/), scheduling cron jobs to run every Tuesday at 12:00 AM, using the correct naming convention for backup files, and verifying backup integrity. Proper documentation and logs are necessary. 

## step-1 : created apache_backup.sh in /usr/local/bin directory

## step-2 : vi apache_backup.sh written code for taking backup of /etc/httpd and /var/www/html and modified the permission using command- sudo chmod +x /usr/local/bin/apache_backup.sh the screenshot of apache_backup.sh file snapshot is following 

![Task 3 Backup Configuration for Web Servers snapshot1](https://github.com/user-attachments/assets/18ba9464-6ba3-4bfa-8935-141481c72f27)

Script for Mike (Nginx)

step-1 : created nginx_backup.sh in /usr/local/bin directory

step-2 : vi nginx_backup.sh written code for taking backup of /etc/nginx /usr/share/nginx/html and modified the permission using command- sudo chmod +x /usr/local/bin/nginx_backup.sh the screenshot of apache_backup.sh file snapshot is following 

![Task 3 Backup Configuration for Web Servers snapshot2](https://github.com/user-attachments/assets/549af7d9-667a-499e-8636-9299b2d71093)

step-3 : after successfull run of both the scripts this is the output screenshot of /backups directory command to verify is - ls -lh

![Task 3 Backup Configuration for Web Servers snapshot3](https://github.com/user-attachments/assets/a4bfa766-f0f3-45aa-bcf2-14dd7aa8f6e9)

step-4: scheduling task to run at every Tuesday at 12:00 AM for sarah's task of backing up apache server configuration: command crontab -e and then write 0 0 * * 2 /usr/local/bin/apache_backup.sh

![Task 3 Backup Configuration for Web Servers snapshot4](https://github.com/user-attachments/assets/aaee0e40-1687-471b-bee9-f996e430a613)

for mike task of backing up nginx server configuration: command crontab -e and then write 0 0 * * 2 /usr/local/bin/nginx_backup.sh

![Task 3 Backup Configuration for Web Servers snapshot5](https://github.com/user-attachments/assets/5782700c-8c3b-4eac-af2a-7c2c4b6e845c)

then verified cronttab tasks with command crontab -l 

![Task 3 Backup Configuration for Web Servers final snapshot](https://github.com/user-attachments/assets/0b67ed91-a7d3-4151-8ba2-6b8d924ce13d)

## Practice Assignment on Testing, Linux and Servers has been completed.




