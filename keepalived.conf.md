## _keepalived configuration_

* Keepalived is a piece of software which can be used to achieve high availability by assigning two or more nodes a virtual IP and monitoring those nodes, failing over when one goes down. Keepalived can do more, like load balancing and monitoring, but this tutorial focusses on a very simple setup, just IP failove

**_requirments_**



**_Installation process_**

---

~~~bash

sudo apt-get update
sudo apt-get install linux-headers-$(uname -r)
sudo apt-get install keepalived

~~~

**_Configuration_**

Now create or edit Keepalived configuration `/etc/keepalived/keepalived.conf` file on LB1

~~~bash
vim /etc/keepalived/keepalived.conf
~~~

Configuration File for MASTER FILE

~~~bash



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


---

Configuration File for backup file


~~~bash


vrrp_instance VI_1 {
    state backup
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

**Start keepalived service**
~~~bash

# sudo systemctl status keepalived
sudo service keepalived start

~~~

