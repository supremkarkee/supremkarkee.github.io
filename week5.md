# ASSESSMENT WEEK 5

# Implementing Access Contorl Using AppArmor

In ubuntu we use AppArmor to implement Access control. 

### AppArmor Status

We can check the status of apparmor by using sudo aa-status


![](/week5_images/aa-status.png)


### Different Modes of Viewing a Profile

sudo aa-status --profiled
sudo aa-satus --enforced
sudo aa-status --complaining

![](/week5_images/complaining.png)


### Difference between enforce mode and complain mode


### 1 . Enforce Mode

### What it does : 
1. Blocks unauthorized actions
2. logs the denied action 
3. Restricts the programs behaviour
4. Provides real security enforcement

### 1. Complain Mode

It does not block anything but will record the log.

### What it does: 

1. Logs every violation
2. helps user understand what the application needs
3. helps refine the profile before enforcing it

### Structure of AppArmor Profile. 

![](/week5_images/aa-profile.png)

AppArmor profiles are simple text files located in /etc/apparmor.d/. They define what a specific program is allowed to do; anything not explicitly allowed is denied.


### Key Components of a Profile

1. Include Files : lines starting with #include and it pulls common variables like where a user home directory is located. 

2. Profile Name : Usually the full path of the program. 

3. Capabilites :specific Linux root privileges the program needs, such as capability setgid (changing group ID) or capability net_bind_service (binding to network ports).

4. Access Modes (Permissions)
r : read
w : write
x : Execute
m : Memory map


## Tracking and Reporting of AppArmor

### Par A : Current State

We first focus on the things that are active and running. 
To do this we need to run a command : 

sudo aa-status --verbose > apparmor_config_report.txt

this command will save the output in a text file. 

we can verify the report using cat apparmor_config_report.txt

### PART B : Reporting 

AppArmor logs "DENIED" messages to the system logs. This command extracts only those security events and saves them to a report file.

we can track this violation using the command : 

grep "apparmor=\"DENIED\"" /var/log/syslog > apparmor_incident_report.txt


## Automatic Security Updates

We can configure automatic updates using the package called unattended-upgrades

installation : sudo apt install unattended-upgrades

configuraton : sudo dkpg-reconfigure --priority=low unattended-upgrades.

![](/week5_images/unattended-upgrades%20installation.%20.png)

### Verification : 

verification can be done using cat /etc/apt/apt.conf.d/20auto-upgrades

![](/week5_images/v-updates.png)

![](/week5_images/verification2_of_autoaupdates.png)


## Configuration of fail2ban

fail2ban is an IPS tools which allows us to implement actions and block unauthorized actions on our system. Unlike an IDS which only detect but takes no action fail2ban actively takes actions against unauthorized activities. 

Installation: 

Instllation is very simple, we need to use a command : 

sudo apt install fail2ban -y

![](/week5_images/fail2ban(1).png)

Configuration: 

After installing the tool we need to create a configuration file 

sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

we first copy a file in a new file 

we then edit the file : sudo nano /etc/fail2ban/jail.local

![](/week5_images/fail2ban(2).png)

then we need to find the sshd section and make these changes : 

before : 

![](/week5_images/fail2ban(3).png)


after : 

![](/week5_images/fail2ban(4).png)



verification: 

Before we verify we need to restart the service using the command :
sudo systemctl retstart fail2ban
sudo systemctl enable fail2ban

this will restart the service and enable it on boot. 

![](/week5_images/fail2ban(5).png)



## security Baseline Script

We need to create a script that checks the security configuration that we established in previous weeks(Phase 4 and 5 )

Security Baseline script has multiple functions and each function is responsible for checking a particular type of security configuration : 

```bash
!#/bin/bash             #shebang



# we will create multiple functions and each function will do a  specific security check 

#fuction to check password authentication grep -q silently checks without printing on the scrren

check_password_auth() {

        if grep -q "^PasswordAuthentication no" /etc/ssh/sshd_config; then
                echo -e "[OK] PasswordAuthentication is disabled."
        else
                echo -e "$[WARNING] PasswordAuthentication is diabled."
        fi
}


# this function check if root login is enabled.
check_root_login() 
{
        if grep -q "^PermitRootLogin no" /etc/ssh/sshd_config; then
                echo -e "[OK] Root Login is Disabled. "
        else
                echo -e "[WARNING] Root Login is Enabled "
        fi
}
#This function is very simple and all it does is check if fail2ban is active.

check_fail2ban_status()
{
        #The '--quite' flag supresses the output so that we can handle
        #with our own echo statements.
        if systemctl is-active --quiet fail2ban; then
                echo "[OK] Fail2ban is active and running"
        else
                echo "[warning] fail2ban is not running"
        fi
}

# this function checks  apparmor status
check_aa_status()
{
        #checks if apparmor is active
        if systemctl is-active --quiet apparmor; then
                echo "[OK] Apparmor is active and running"
        else
                echo "[WARNING] Apparmor is not running"
        fi
}
# this function is just for printing the header.
print_header() {

        echo -e " === SECURITY BASELINE CHECK === "
}


check_autoupdates()
{


        #check if the service is active and has "Update-package-Lists" set to "1"
        #check if the config file exits.

        if systemctl is-active --quiet unattended-upgrades && \
                grep -q 'APT::Periodic::Update-Package-Lists "1";'  /etc/apt/apt.conf.d/20auto-upgrades 2>/dev/null; then
                echo "[OK] unattended upgrades are active and configured."
        else
                echo "[WARNING] automatic updares are not properly configured"
        fi
}

main()
{
        print_header
        check_password_auth
        check_root_login
        check_firewall
        check_ip_port_allowed "192.168.10.3" "2424/tcp"
        check_fail2ban_status
        check_aa_status
        check_autoupdates

}

main
```

## Monitoring Script 

We will use a very simple script to monitor our server. 

The script will do the following : 
1. SSH in to the server
2. display information such as hostname, uptime, memory usage, disk usage, and other active processes. 


## script for monitoring 
``` bash

#!/bin/bash

# Configuration
REMOTE_USER="sausage"      # <--- server username
REMOTE_HOST="192.168.10.4"      # <--- server ip address


# below lines prints user name and host ip 
echo "----------------------------------------------------"
echo "Starting Remote Monitor for: $REMOTE_USER@$REMOTE_HOST"
echo "----------------------------------------------------"

# below line performs the ssh and uses EOF to pass multiple commands. 
ssh -p 22 -T $REMOTE_USER@$REMOTE_HOST << 'EOF'

    echo ">>> SYSTEM IDENTITY"
    hostname
    echo ""

    echo ">>> MEMORY USAGE (MB)"
    free -m
    echo ""

    echo ">>> DISK USAGE"
    df -h /
    echo ""

    echo ">>> TOP 5 CPU PROCESSES"
    # We use 'ps aux' sorted by CPU usage. 
    # This is more compatible across different Linux versions.
    ps aux --sort=-%cpu | head -n 6
    echo ""

EOF

echo "----------------------------------------------------"
echo "Monitoring Complete."
echo "----------------------------------------------------"


```


