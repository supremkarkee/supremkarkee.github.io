# Assessment Week 2 : Security Plannning and Testing Methodology

## 1. Performance Testing Plan
### **Objective : ** To design a methodology for monitoring system resources and stress-testing process management controls.

## Remote Monitoring Methodology

I have implemented an agentless monitoring approach. Instead of installing heavy GUI applications on the server, I utilized standard CLI tools accessed remotely via SSH.

## Tools Used
### top 
 interactive process viewer for linux, which shows real-time system resource usage (CPU, RAM, tasks, Load etc.)


### Usage Syntax

**top** This displays CPU memory Usage and other system information


**P** Inside top, when pressed 'P' will display and sort processes by CPU usage.

**M** Similar to 'P', 'M' will display Memory usage

Note : You can use the htop/btop tool for more interactive version of top.

### ps aux 
 displays snapshot of all running processes with detailed information 

 ### Usages Syntax

 **ps aux** shows all process 
 **ps aux --sort=-%mem**  sort by memory usage '-' shows order in descending order
 **ps aux --sort=-%cpu** sort by CPU usage.
 **ps -u username** shows process from a specific user.
 **ps -p <pid>** Extract process by process id.
 **pstree -p** show tree view or hierarchy (pstree)

### nmon
Advanced system monitoring tool with export feature for data anaysis.

### Usage Syntax

**nmon [options]**

### Options

**-o <file>**  specify output file
**-t** top processes
**-a** capture all stats
**-D** Disk stats
**-N** Network stats
**-C** CPU stats
**-M** Memory Stats
 ## Testing Approach

 To validate the effectiveness of process management controls, I followed a structured testing lifecycle.

 ## 1. Idle state

This is the idle resource consumption by server.

### Idle state using top 

![System idle from top ](images-week2/server-idle-1.png)

### Idle state using htop

![System idle from htop ](images-week2/server-idle-2.png)

### PS tree

![System ps tree](images-week2/server-pstree.png)

### btop

**Installation**
sudo apt install btop

![image proof of installing btop](/images-week2/btop-installation.png)

**System idle state from Btop**

![System idle state from btop](/images-week2/btop-idle.png)

### nmon

**Installation**
sudo apt install nmon

![nmon installation](/images-week2/nmon.png)

**nmon menu page**  nmon -a

![nmon installation](/images-week2/nmon-s1.png)

**nomon view with CPU, Memory and network status**

![nmon installation](/images-week2/nmon-view.png)

## Testing Approach

For testing I have decided to use tools called stress. This tool can generate load on the system which we can monitor using the tools already described above. Stress tool can be installed using the command sudo apt install stress

Note : for powerful stress tool we can use stress-ng

### Using stress tool.

command given = stress --cpu 4 --timeout 60&

### Results from monitoring tools

**top**
![top](/images-week2/stress-in-top.png)

**btop**
![btop](/images-week2/stress-in-btop.png)

**nmon**
![nmon](/images-week2/stress-in-nmon.png)

**psstree**
![psstree](/images-week2/stress-in-pstree.png)





