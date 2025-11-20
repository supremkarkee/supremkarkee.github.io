# Phase 1: System Planning and Distribution Selection

## 1. Introduction
[cite_start]This document outlines the planning and justification for a virtual operating system deployment[cite: 1, 2]. [cite_start]The main objective is to select and deploy two operating systems in a networked environment which will act as a foundation for Phase 2 and Phase 3. This report details the system architecture, the justification for the selected OS for the server and workstation, as well as the virtual network connection[cite: 3].

### Operating Systems Selected
* [cite_start]**Server:** Ubuntu Server 24.04.3 TLS [cite: 5]
* [cite_start]**Workstation:** Ubuntu 24.04.3 Desktop [cite: 6]

---

## 2. Server Distribution Selection & Justification
[cite_start]**Selected Distribution:** Ubuntu Server 24.04.3 TLS [cite: 8]

The server is the backbone of this project. [cite_start]Stability, reliability, and system updates are the most important considerations for choosing this operating system[cite: 9].

### Primary Justification

#### A. Stability
It is based on Debian Linux and has a reputation for being one of the most stable and secure operating systems available. [cite_start]The Long Term Support (LTS) releases of Ubuntu Server receive standard security updates for around 2,500 packages in the Ubuntu Main repository for five years by default[cite: 12]. [cite_start]This makes the operating system highly reliable and secure[cite: 13].

#### B. Security
Each LTS release comes with five years of free standard security maintenance from Canonical. [cite_start]Ubuntu also comes with several key security mechanisms like **AppArmor**, **Uncomplicated Firewall (UFW)**, and a **Hardened Default Configuration** (by default, no ports are open, and root login is locked)[cite: 15, 16].

#### C. Performance
[cite_start]Ubuntu ships with a modern version of the Linux kernel which provides a significant performance advantage by including the latest drivers for modern CPUs, NVMe storage, and networking hardware[cite: 18]. [cite_start]It comes without a GUI (Headless), which keeps the system lean with minimal usage of RAM and CPU[cite: 19]. [cite_start]It is also highly optimized, making it a leading platform for Docker and Kubernetes[cite: 20].

#### D. Simplicity and Connectivity
Ubuntu Desktop and Ubuntu Server share the same underlying architecture and CLI. [cite_start]This means the same commands can be used across both systems, making it easier to switch between environments and manage them efficiently[cite: 22].

### Comparison with Alternatives

| Feature | **Ubuntu Server** | **CentOS** | **Fedora OS** | **Windows Server** |
| :--- | :--- | :--- | :--- | :--- |
| **Type** | Free, Debian open-source | Free, open-source | Free, open-source | Expensive, Closed-source |
| **Updates** | **LTS Release** (5 years support, stable) | **Rolling-release** (continuous updates) | **Short Lifecycle** (13 months, disruptive updates) | **2-3 Year Cycle** (2016, 2019, 2022, etc.) |
| **Usability** | Widely considered the friendliest Linux server. | Not as simple; intended for developers. | Aimed at experts comfortable with frequent changes. | GUI is simple, but automation/scripting is complex. |
| **Security** | Based on **AppArmor** (Simple, user-friendly). | Based on **SELinux** (Powerful but complex). | Relies on SELinux. | Proprietary closed-source model. |
| **Package Mgr** | **APT** (.deb) - Largest repo in the world. | DNF/YUM (.rpm) | DNF/YUM (.rpm) | Fragmented (GUI, PowerShell, 3rd party). |

[cite_start][cite: 24]

---

## 3. Workstation Distribution Selection & Justification
[cite_start]**Selected Distribution:** Ubuntu Desktop 24.04.3 [cite: 25]

[cite_start]Ubuntu Desktop is one of the most popular and user-friendly open-source operating systems[cite: 26]. [cite_start]It is widely used within the development community and benefits from strong, community-driven support[cite: 27].

### Primary Justification

* [cite_start]**Accessibility:** The installer is simple, hardware detection is excellent, and the GNOME desktop is intuitive for users coming from Windows or macOS[cite: 30].
* [cite_start]**Lightweight:** It does not come pre-installed with unnecessary bloatware[cite: 32].
* [cite_start]**Stability (LTS):** Guarantees five years of free security and maintenance updates, making it perfect for users who need a reliable system without constant major updates[cite: 34, 35].
* **Community Support:** Extensive documentation is available. [cite_start]Solutions to almost any problem can be found on GitHub or Stack Overflow[cite: 37].
* **Security:** For general-purpose desktops, it is highly secure. [cite_start]It uses a robust permission system (root locked by default), AppArmor, UFW, and disk encryption[cite: 39, 40, 41, 42].

### Comparison with Alternatives

| Feature | **Ubuntu** | **Windows 11** | **macOS** | **Fedora Workstation** |
| :--- | :--- | :--- | :--- | :--- |
| **Cost** | Free | Paid Licensing | Hardware Dependent | Free |
| **Interface** | GNOME (Highly Customisable) | Customised (Limited) | Aqua (Low Customisation) | GNOME (Stock) |
| **Support** | **5 Year LTS** | Long term | Tied to Hardware | Short (13 months) |

[cite_start][cite: 44]

---

## 4. Network Configuration
**Network Type:** NAT Network
**Network Name:** `OSnetwork`
[cite_start]**Status:** Both OSs are isolated but can communicate with each other[cite: 46, 47].

![Network Settings in VirtualBox](./images/network-settings.png)
*[Ref: VirtualBox Network Settings]*

### IP Address Configuration

**1. [cite_start]Server IP Address:** `192.168.10.4 /24` [cite: 49]
![Server IP Address Output](./images/server-ip.png)

**2. [cite_start]Workstation IP Address:** `192.168.10.3 /24` [cite: 51]
![Workstation IP Address Output](./images/workstation-ip.png)

### Connectivity Tests (Ping)

**From Server to Workstation:**
![Ping from Server to Workstation](./images/ping-server-to-workstation.png)
[cite_start]*[Result: 0% Packet Loss, successful connection]* [cite: 53]

**From Workstation to Server:**
![Ping from Workstation to Server](./images/ping-workstation-to-server.png)
[cite_start]*[Result: 0% Packet Loss, successful connection]* [cite: 54]

---

## 5. System Documentation (CLI Verification)

I used command-line tools to verify the system specifications for both machines.

### A. Workstation Documentation

**1. Kernel Version (`uname`)**
[cite_start]This command confirms the machine is running the Linux kernel[cite: 57].
![Workstation Uname Output](./images/workstation-uname.png)

**2. Memory Usage (`free -h`)**
Total RAM allocated is **8GB** (8131028 KB). [cite_start]Currently, around 1GB is used, leaving 6GB free[cite: 58, 60, 61].
![Workstation Free Memory Output](./images/workstation-free.png)

**3. Disk Usage (`df -h`)**
The main partition (`/dev/sda2`) is **25GB**, with 5.9GB used (26%). [cite_start]The `tmpfs` filesystems (Run/Lock/Shared Memory) are temporary storage in RAM and will be cleared upon reboot[cite: 62, 63, 65].
![Workstation Disk Usage Output](./images/workstation-disk.png)

**4. Distribution Info (`lsb_release -a`)**
[cite_start]Confirms the OS is **Ubuntu 24.04.3 LTS** (Codename: noble)[cite: 66, 68].
![Workstation LSB Release Output](./images/workstation-lsb.png)

---

### B. Server Documentation

**1. Kernel Version (`uname`)**
![Server Uname Output](./images/server-uname.png)
[cite_start]*[Result: Linux]* [cite: 70]

**2. Memory Usage (`free -h`)**
Total RAM is **9.6GB**. Currently used: **419MB**, Free: **9.5GB**. [cite_start]This confirms the headless server uses significantly less RAM than the desktop version[cite: 72, 73].
![Server Free Memory Output](./images/server-free.png)

**3. Disk Usage (`df -h`)**
The main drive (`/dev/sda2`) is **25GB** with only **2.7GB** used (12%). [cite_start]The server installation is much smaller than the workstation[cite: 74, 76].
![Server Disk Usage Output](./images/server-disk.png)

**4. Distribution Info (`lsb_release -a`)**
[cite_start]Confirms the Server is running **Ubuntu 24.04.3 LTS** (Codename: noble)[cite: 78, 80].
![Server LSB Release Output](./images/server-lsb.png)