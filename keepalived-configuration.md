## _keepalived configuration_

Keepalived is a piece of software which can be used to achieve high availability by assigning two or more nodes a virtual IP and monitoring those nodes, failing over when one goes down. Keepalived can do more, like load balancing and monitoring, but this tutorial focusses on a very simple setup, just IP failove


![image](https://assets.digitalocean.com/articles/high_availability/ha-diagram-animated.gif)


**_requirments_**

|IP|OS|HOSTNAME|
|---|---|---|
|172.31.30.178/20|Ubuntu|ha-proxy-a|
|172.31.31.158|Ubuntu|ha-proxy-b|
|172.31.31.300|Ubuntu|virtual-ip|


## Check whether we installed the service in the system before installing the packages

~~~bash

dpkg --list | grep nginx

~~~

**_Installation process_**

---

~~~bash

sudo apt-get update
sudo apt-get install keepalived -y

~~~

**_Configure KeepAlived_**

Once KeepAlived package is installed, create the main configuration file /etc/keepalived/keepalived.conf with below configuration. Replace the Highlighted values as per your configurations.

~~~bash

vim /etc/keepalived/keepalived.conf

~~~

Configuration file for MASTER file

~~~bash

# configuation for the virtual interface

vrrp_instance VI_1 {
    state MASTER
    interface wlp1s0
    virtual_router_id 101
    priority 101
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.10.105/32
    }
}

~~~

**Use systemctl command to start and enable the KeepAlived service as below.**
~~~bash

 systemctl restart keepalived
 systemctl enable keepalived
 
~~~


----


Configuration file for backup file


~~~bash

# configuation for the virtual interface

vrrp_instance VI_1 {
    state BACKUP
    interface wlp1s0
    virtual_router_id 101
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.10.105/32
    }
}

~~~

**Start keepalived service**
~~~bash

# sudo systemctl status keepalived
sudo service keepalived start

~~~

