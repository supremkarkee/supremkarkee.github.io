

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

After these steps we need to configure the file : /etc/ssh/sshd_configa

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









