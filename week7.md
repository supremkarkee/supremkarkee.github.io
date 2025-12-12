# ASSESSMENT WEEK 7 


## 1. Security Scanning with Lynis

### Tools Installation 

```bash
sudo apt install lynis nmap -y 
# this will install lynis and nmap 
```

### Tools Verification 

``` bash 
namp --version 
lynis --version
```

### Auditing the system using lynis

We can audit the system using the tool lynis using this command : 

``` bash 
sudo lynis audit system 

# we will need sudo permission for this to work 
```

### Running the audit test 

![](/week7_images/auditing.png)


NOTE : Please note that the testing time could take few minutes. 


### Audit score Before


![](/week7_images/audit_score.png)

### audit warning and suggestions.

![](/week7_images/suggestion(1).png)

solution : 

``` bash 
sudo apt update
sudo apt install apt-listbugs

# It runs automatically during apt upgrade/apt install, showing serious bug reports before packages are installed.

# we can also check manually using : 

apt-listbugs list package-name

```

![](/week7_images/suggestion(2).png)


solution: 

``` bash 
sudo apt install apt-listchanges

#It runs automatically during apt upgrade/apt install, showing important upstream changelogs before packages are installed.


```

![](/week7_images/suggestion(ssh).png)

SSH hardening based on suggestions. 


### SSH hardening based on suggestions. 

AllowTcpForwarding no
ClientAliveInterval 300
ClientAliveCountMax 2
LogLevel VERBOSE
MaxAuthTries 3
MaxSessions 2
TCPKeepAlive no
X11Forwarding no
AllowAgentForwarding no


Description : This configuration disables TCP forwarding to prevent SSH tunneling abuse, sets ClientAliveInterval and ClientAliveCountMax so idle sessions disconnect automatically, raises LogLevel to VERBOSE for detailed auditing, limits MaxAuthTries to reduce password-guessing attempts, restricts MaxSessions to control how many channels a user can open, disables TCPKeepAlive to avoid keeping dead connections open, and turns off both X11Forwarding and AllowAgentForwarding to block unsafe GUI and agent-credential forwarding.

![](/week7_images/suggestion(malware).png)


solution: 

``` bash
sudo apt install rkhunter
sudo rkhunter --update
sudo rkhunter --propupd

#installs a malware scanning tool.
```

we can also use and configure daily corn job to  automate scanning time to time. 

we can do this using the command : 

``` bash 
sudo nano /etc/default/rkhunter

# Make this change inside the file : 

CRON_DAILY_RUN="yes"
CRON_DB_UPDATE="yes"

```

### Audit Score after making some changes: 

![](/week7_images/after_audit.png)

## Network Security Verification 

### Firewall rule

![](/week7_images/firewallrule.png)

### result of nmap 

![](/week7_images/nmap(2).png)

Port 2424 is opened for remote ssh , during the ssh hardening phase we changed and configured the firewall as well as sshd_config file to allow ssh from the port 2424.


### Service Inventory & Justification

![](/week7_images/listeningservice.png)



| Service | Port/Proto | Listening On | Justification & Status |
| :--- | :--- | :--- | :--- |
| **systemd-resolve** | 53 (TCP/UDP) | Localhost | **Essential:** Local DNS resolver required for system domain lookups. Secure (listening locally only). |
| **systemd-network** | 68 (UDP) | Internal IP | **Essential:** DHCP client required to maintain network connectivity and IP address assignment. |
| **sshd** | 2424 (TCP) | All Interfaces | **Critical:** Remote administration access. **Hardened:** Default port 22 changed to 2424 to reduce automated attacks. |
| **apache2** | 80 (TCP) | All Interfaces | **Primary Service:** Web server hosting the required web application. |
| **iperf3** | 5201 (TCP) | All Interfaces | **Testing Tool:** Active for performance benchmarking. *Recommendation: Disable when testing concludes to reduce attack surface.* |





# Problem faced during this week 

System crashed multiple times and boot load failed. 

fix : play around the resource allocation for example if you have allocated 8 cores then change it to 4 or reallocate more RAM. 


## Network Interface crashed completely

My default network interface enp03s got crashed and I was not able to ping or make any connections. I tried to bring the interface up by
using the command sudo ip link set enp03s up but it did not worked . Upon further investigation and rebooting system multiple times I changed the adapter and applied the same settings : 

![](/week7_images/networkproblem.png)

after these changes I got a new IP which I then configured and setup firewall rules to allow new IP for ssh. 


