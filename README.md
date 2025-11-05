# SDLC-Testing-Linux-and-Server-assignment
## HeroVired Linux assignment
## Task 1: System Monitoring Setup
## Task 1 Summary:
Install and configure htop or nmon to monitor CPU, memory, and processes, using df and du for disk usage tracking, and identifying resource-intensive processes. Proper logging of system metrics and clear documentation of the setup are essential. 

## Task 1: System Monitoring Setup Lab resluts:

# first step: create system_monitor.sh script in /usr/local/bin/ folder

# second-step: Make it executable: sudo chmod +x /usr/local/bin/system_monitor.sh

# third-step: scheduled it for every hour: echo "0 * * * * root /usr/local/bin/system_monitor.sh" | sudo tee -a /etc/crontab

# fourth step : after successful run script  will generate log in as /var/log/system_monitor_yyyy-mm-dd.log file


## Tools Installed
# - top (CPU, Memory, Process)
# - df, du (Disk usage monitoring)

## Automated Logs
# - /var/log/system_monitor_YYYY-MM-DD.log

# result after successful run

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2d827b9c-bdd1-4784-b6eb-72c1201c72d2" />

## Task 2: User Management and Access Control
## Task 2- Summary
Evaluation includes creating user accounts for Sarah and Mike with secure passwords, setting up isolated directories with appropriate permissions, and enforcing a password policy with expiration and complexity. Detailed documentation of user management steps is required. 

## Task 2: User Management and Access Control lab resluts:
tep 1: Create User Accounts
#    Log in as root or a sudo user:
#   sudo su -

#    Create users Sarah and Mike:

#    useradd -m -d /home/Sarah -s /bin/bash Sarah
#    useradd -m -d /home/mike -s /bin/bash mike

#    -m: creates the home directory

#    -d: specifies the home directory path

#    -s: assigns the default shell (bash)


#    securing passwords:
#    passwd Sarah
#    passwd mike

# step 2 :
#  Create Isolated Workspaces

# Create working directories:
# mkdir -p /home/Sarah/workspace
# mkdir -p /home/mike/workspace

# Assign ownership:
# chown Sarah:Sarah /home/Sarah/workspace
# chown mike:mike /home/mike/workspace

# Set permissions to restrict access:
# chmod 700 /home/Sarah/workspace
# chmod 700 /home/mike/workspace
# Permission breakdown for 700:

# Owner: Read, Write, Execute

# Group: No access

# Others: No access


# Step 3: Verify Access Control:
# su - Sarah
# cd /home/mike/workspace  # Should get "Permission denied"
# exit

# su - mike
# cd /home/Sarah/workspace  # Should get "Permission denied"

# Step 4: Implement Password Expiration and Complexity Policies
# Configure Password Expiration:
# Set password expiry to 30 days:
# chage -M 30 Sarah
# chage -M 30 mike

# To verify:
# chage -l Sarah
# chage -l mike


# Enforce Password Complexity
# sudo nano /etc/security/pwquality.conf

## Task 3: Backup Configuration for Web Servers
## Task3: Summary:

Configure automated backups for Apache (/etc/httpd/, /var/www/html/) and Nginx (/etc/nginx/, /usr/share/nginx/html/), scheduling cron jobs to run every Tuesday at 12:00 AM, using the correct naming convention for backup files, and verifying backup integrity. Proper documentation and logs are necessary. 

Overall Report and Presentation

The report should clearly summarize implementation steps and challenges, including relevant screenshots or terminal outputs demonstrating task completion.

## Task 3: Backup Configuration for Web Servers lab results.

 ## Backup
 










