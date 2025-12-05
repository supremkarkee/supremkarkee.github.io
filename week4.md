# ASSESSMENT WEEK 4

## Server hardening

## 1. Configuring ssh for key based authentication.

The whole process of configuring ssh for key based authentication is explained in following steps.

### Step 1 : **SSH key generation**
 Before we configure the server to allow ssh with keys we need to generate ssh key in the workstation. The key can be generated using the command : 
 
   ssh-keygen -t ed25519 -C "comments"


   ![ssh key generation image ](/week4_image/keygen4.png)
    ssh


### Step 2 : ***Installing the key in the server****

   Once the key has been generated we need to install the key to the server we want to access, this can be done using the command : ssh-copy-id username@server_ip_address

   ![Key installation ](/week4_image/key-install.png)

### Step 3 : **Configuration of ssh file**

After these steps we need to configure the file : /etc/ssh/sshd_config

**Make changes to these parameters**

1. PubkeyAuthentication	yes

    ![](/week4_image/pub-key-yes.png)

2. PasswordAuthentication	no

    ![](/week4_image/passauthentication_no.png)


### Step 4 : Logging in.

After these changes we can simply shh into the server without any passwords. 

shh username@ip address of server

   ![](/week4_image/ssh-login.png)


## 2. Configuration of firewall to allow ssh from one specific host only

Using uncomplicated firewall. 

Current status : 

   ![](/week4_image/ufw-current-status.png)


rule 1: ufw allow from workstation_ip_address to any port 22/(custom port if any) proto tcp
rule 2: ufw deny 22/tcp

After applying these rules we should be able to perfrom ssh from one specific ip address only.


Status after changes : 

   ![](/week4_image/status_after_change.png)



## 3. Managing users and creating non-root administrator

Creating a non root administrative user

![](/week4_image/usercreation.png)


User Group Verification 

![](/week4_image/adding_user_to_sudo_verfication.png)


tests

![](/week4_image/switching_user_udate_test.png)

![](id)



## Principle of Least Privilege

Principle of least privilege ensures that a user is granted only a mininum necessary permission to perform a task. Applying this principle helps to secure the system from accidental or malicious 
system changes, specially when using root or administrative access.

## Why Use Non-root administrative user ?

Root access allows a user to enter the root mode (god mode) which provides unrestricted control over the system and can be risky if mishandled. By using non-root administrative user like the adminuser, we can limit the risk of unintended system-wide changes, while still allowing the user to perform administrative tasks when necessary via sudo.


## Remote administrative demonstration. 

![](/week4_image/remoteadmin(1-3).png)


![](/week4_image/remoteadmin(4).png)


![](/week4_image/remoteadmin(5).png)


## SSH connection Demonstration 

![](/week4_image/demo(1).png)


![](/week4_image/demo(2).png)


![](/week4_image/demo(3).png)


![](/week4_image/demo(final).png)

















