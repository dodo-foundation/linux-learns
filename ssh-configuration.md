## ssh configuration

**what is ssh?**

SSH is a client program for logging into a remote Linux/Unix server.ssh port runs on TCP port 22 on Ubuntu Linux

**Requirements**

| SERVER | OPERATING SYSTEM | ADDRESS |
|---|---| --- |
| virtual machine | ubuntu | 192.168.0.2 |

**how to install the ssh packages**

```bash
sudo apt update
sudo apt install openssh-client -y
```


**how to enable ssh server on ubuntu**

```bash
sudo systemctl enable ssh
```
**how to view status of ssh run**

```bash
sudo systemctl status ssh
```
**Once the installation done, use this command to loging to the remote server**

```bash
ssh user@server-ip-here
```

**how do disable the ssh server on ubuntu**

```bash
sudo systemctl stop ssh
sudo systemctl disable ssh
sudo apt-get remove opnessh-server
```
**Limit SSH User Logins**

If you have a large number of user accounts on the systems, then it makes sense that we limit remote SSH access to those users who really need it. Open the /etc/ssh/sshd_config file.

  vim /etc/ssh/sshd_config
  
  Add an AllowUsers line at the bottom of the file with a space separated by a list of usernames.
  
  ![image](https://user-images.githubusercontent.com/98270930/165550413-043b9c7d-9f5b-48cb-95d7-8fe3628c2450.png)


**output**

![image](https://user-images.githubusercontent.com/98270930/165551212-b7e3b4ae-cf10-4f23-836a-8d748d3b9ac5.png)


![image](https://user-images.githubusercontent.com/98270930/165553054-02aab5bb-39c5-49b8-830b-cf061ff51feb.png)

  
  **SSH PORT LISTEN CHECK**
  
  ```bash
  netstat -tulpn
  ```
   **Output**
   ![Screenshot from 2022-04-28 19-09-23](https://user-images.githubusercontent.com/102893121/165765334-a8cba6af-3a11-4bd1-8583-c8f357630a6e.png)


_**SSH Login User Kill Process**_

If Unknown login via ssh to your server frst find ssh connenction and PID then kill the process using below command

```bash

#find unkonwn ip 
ss -tu

#find PID Id
sudo netstat -tnpa | grep 'ESTABLISHED.*sshd'
sudo kill PID

```

  
  ### SSHD config file Important Parameters
  
  **1.check permission for the config file  
  
  **/etc/ssh/sshd.config** files check GID and UID to Root or Use ls -a to view the the file permission to by default 600 
  
  ```bash
 stat /etc/ssh/sshd_config
 ```
 ![Screenshot from 2022-04-29 17-07-48](https://user-images.githubusercontent.com/102893121/165937368-0301e1d8-fa93-47cd-8636-bac7cafaf231.png)
 
 
  **2.Check permission **SSH private and Public host key files**
 
  **Note**
     
     ssh_host_dsa_key => denotes Private key file and check file permission 600 by default
               
     ssh_host_dsa_key.pub => denotes Public key file and check file permission 644 by default

![Screenshot from 2022-04-29 17-19-57](https://user-images.githubusercontent.com/102893121/165938942-9f426465-8a1c-4b74-b13f-9127196d937c.png)
    
  **3.SET USER,GROUP ACCESS permission (LINE NO 86)**
    
    ```bash
     vim /etc/ssh/sshd_config
     ```
  **Insert "Allow User" "Deny User" "Allow Group" "Deny Group"
     
  **output**
 ![Screenshot from 2022-04-29 18-20-30](https://user-images.githubusercontent.com/102893121/165947560-7405ff17-97a4-4e85-a613-719ffb1bc298.png)

  
  **4. check LogLevel (LINE NO:28)**
  
      Two Levels there **INFO AND VERBOSE** 
      
      INFO - Only recors login activity
      
      Verbose - specifies that login and logout activity
  
      
      By default - INFO **If ur wish chage INFO TO VERBOSE**
  
   
   **5.X11 forwarding is disabled(LINEE NO 90)** 
   
      By Default YES **If you chage NO**
   
   
   **6.SSH MaxAuthTries is set to 4 or less (LINE NO 35)**
   
      The MaxAuthTries parameter specifies the maximum number of authentication attempts
      permitted per connection.
   
      By Default 6 **If you chage 4**
   
   
   **7.SSH root login is disabled(LINE NO 33)**
   
      PermitRootLogin no
   
   
   **8. SSH PermitEmptyPasswords is disabled(LINE NO 58)**
   
      By default PermitEmptyPasswords no
   
       
   **9.SSH PermitUserEnvironment is disabled(LINE NO 97)**
   
      By Default PermitUserEnvironment no
   
   
   **10.SSH Idle Timeout Interval is configured(LINE NO 99,100)** 
     
      BY default ClientAliveInterval 0 **If you change 300**
      ClientAliveCountMax 3
   
   
   **11.SSH LoginGraceTime is set to one minute or less(LINE NO 32)** 
   
      BY default LoginGraceTime 2m **if change 60**
     
   
   **12. SSH warning banner is configured (Line No 109)**
   
      By Default None 
   
