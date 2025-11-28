# Assessment Week 3

## Introduction

In this week we will select some applications and tools to generating different types of workload for performance evaluation and we will also provide justification as well as some features provided by the tool along with documentation for installation.

## Application Selection 

### CPU-intensive

For generating workloads that are CPU intensive there are several tools such as stress, stress-ng, sysbench, openssl, and prime95/mprime.

We have chosen sysbench over other alternatives because it is based on prime number calculation, mathematically consistent processor load.

### Intallation

This tool can be installed using sudo apt install sysbench -y.

verification for installation can be done using

sysbench --version

![sysbench version](/week3_image/w1.png)



### RAM intensive

For performance testing on RAM we have tools like stress, stress-ng and memtester.

We will go with stress-ng because it is also an upgraded version of stress and we can also perform agressive cycle memeory allocation testing.

### Installation

Installation can be done by using the command :

sudo apt install stress-ng -y

Verification : stress-ng --version

![stress-ng](/week3_image/w2.png)


### I/O Intensive

There are alterantive tools like stress, stress-ng, dd, and fio. We will select fio because it is highly flexible and configurable. It also allows us to simulate specific database patterns versus file server patterns and bypasses the OS cache for true hardware speed.

### Installation

This tool can be installed using the command:

 sudo apt install fio -y


Verification:

fio --version

![fio ](/week3_image/w3.png)

### Network Intensive.

we will choose  iperf3 over other alternative tools.

Some of the alternative tools that were considered are ping,mtr,netperf,nload,ethtool,netcat,hping3, pingpong and bmon.

The reason why we chose iperf3 over other alternatives is because of the following ways:

1. It generates pure TCP/UCP traffic between client and server to measure maximum throughput, jitter, and packet loss.

2. It is completely independent of hard drive speed.

3. It is best for testing pipe capacity.


### Installation.

It can be installed with :

sudo apt install iperf3 -y

Verification:

iperf3 --version.


![iperf3](/week3_image/w4.png)

## Application Selection Matrix

The following matrix details the tools selected to generate specific workloads for performance evaluation, along with the justification for their selection over alternative options.

| Workload Type | Selected Application | Alternatives Considered | Justification for Selection |
| :--- | :--- | :--- | :--- |
| **CPU-Intensive** | **Sysbench** | stress, stress-ng, openssl, prime95 | Chosen for its mathematical consistency (prime number calculation). It provides a stable, repeatable processor load compared to simple "stress" tools, making it ideal for benchmarking. |
| **RAM-Intensive** | **Stress-ng** | stress, memtester | We selected `stress-ng` as it is a significantly upgraded version of `stress`. It allows for aggressive cycle memory allocation testing and can force virtual memory thrashing to test swap performance. |
| **I/O-Intensive** | **Fio** | dd, stress, stress-ng | `fio` is preferred because it is highly flexible and configurable. It allows us to simulate specific database patterns versus file server patterns and can bypass the OS cache to measure true hardware speed. |
| **Network-Intensive** | **iPerf3** | ping, netcat, nload, hping3 | Best for testing pipe capacity. Unlike file transfer tools, `iperf3` is independent of hard drive speeds and measures pure TCP/UDP throughput, jitter, and packet loss. |




## Monitoring Strategy 

To measure the performane impact of the selected applications, we will use utilize the tools which we established in week2.



## Expected Resource Profiles

Based on the selected applications, we anticipate the following resource usage patterns during the testing phase:

### 1. CPU Evaluation (Sysbench)
We anticipate a resource profile showing **100% utilization** of all assigned cores (User CPU time). The system Load Average should rise to match the number of vCPUs. Memory and Disk usage will remain negligible during this specific test.

**test**
command given : sysbench cpu --cpu-max-prime=20000 run

![sysbench command](/week3_image/w5.0.png)



![output sysbench](/week3_image/sysbench_output.png)

### 2. RAM Evaluation (Stress-ng)
We expect RAM usage to spike rapidly to the limit. Once physical RAM is full, we expect **Disk I/O** to increase significantly as the OS begins **swapping** (moving inactive pages to disk to prevent crashing). You may see `kswapd` processes consuming CPU cycles during this phase.

**test**

command given : stress-ng --vm $(nproc) --vm-bytes 90% --timeout 20s

![stress-ng](/week3_image/w6.0.png)


![stress ng output](/week3_image/stres-ng-o1.png)

![stress ng output](/week3_image/stress-ng2.png)

![stress ng output](/week3_image/stress-ng3.png)


### 3. Disk I/O Evaluation (Fio)
The Disk Read/Write speeds should hit the drive's physical or throttled limit. Consequently, CPU usage will likely appear as high **"iowait"** (the time the CPU spends waiting for the disk to complete operations) rather than active processing.


![image proof](/week3_image/w7.0.1.png)


![image proof](/week3_image/w7.proof.png)


![image proof](/week3_image/w7.btop.png)

![image proof](/week3_image/w7.htop.png)



### 4. Network Evaluation (iPerf3)
CPU and RAM usage are expected to remain low. The primary metric will be network throughput, which is expected to plateau at the limit of the virtual **Network Interface Card (NIC)** or the bandwidth cap provided by the hosting provider.

**Rules for testing using iperf3**

1. server and both client (Desktop) must have installed iperf3.
2. server must be enabled for listening. (iperf3 -s).
3. Client can generate the network traffic using iperf3 -c <serverip> 
4. ufw must be configured to allow the network traffic which can be done using sudo ufw allow <port>/tcp (udp).


![server side listening](/week3_image/server_side_listening.png)


![server side listening](/week3_image/server_side_listening_test.png)


![client side listening](/week3_image/client_side_testing.png)


![client side listening](/week3_image/client-side-packet-sending-proof.png)


## Monitoring network test using nmon

![nmon network monitoring](/week3_image/network_testing_proof1.png)


![nmon network monitoring](/week3_image/network_testing_proof2.png)



## Monitoring Strategy 

To measure the performane impact of the selected applications, we will use utilize the tools which we established in week2.




