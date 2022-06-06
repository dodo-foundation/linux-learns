## haproxy configuration

haproxy configuration using the purpose for load balancing and high availability and proxy functionality. haproxy is particularly suited for very high traffic websites and is therefore often used to improve web service reliability and performance for multi-server configurations

**Examination of requiremnets**

|SERVER| IPADDRESS|OS|
|---|---|---|
|haproxy-a| 192.168.0.2| ubuntu |
|haproxy-b| 192.168.0.3| ubuntu |

![image](https://assets.digitalocean.com/articles/high_availability/ha-diagram-animated.gif)




**server installtion**

```bash
sudo apt update 
sudo apt install -y haproxy

```

**HAProxy version**

```bash
haproxy -v
```

**Configuraton**

Configuring Load balancer on Layer 1

```bash
# sudo vi /etc/haproxy/haproxy.cfg


frontend http_front
   bind *:80
   stats uri /haproxy?stats
   default_backend http_back

backend http_back
   balance roundrobin
   server <server1 name> <private IP 1>:80 check
   server <server2 name> <private IP 2>:80 check
```

  layer 4 load balancer with a front-end name http_front listening to the port number 80, which then directs the traffic to the default backend named http_back
  The additional stats URI /haproxy?stats enables the statistics page at that specified address.
    
  After making the configurations
  sudo systemctl restart haproxy
  Testing the setup
    
 http://<public IP>/haproxy?stats
   
 # HAProxy Log setup
   
 log 127.0.0.1 local Needed to enable the UDp SYSLOG reception in /etc/rsyslog.conf
 vi /etc/rsyslog.conf
 #### MODULES ####

$ModLoad imuxsock # provides support for local system logging (e.g. Via logger command)
$ModLoad imklog # provides kernel logging support (previously done by rklogd)
#$ModLoad immark # provides --MARK-- message capability

# Provides UDP syslog reception
$ModLoad imudp
$UDPServerRun 514

# Provides TCP SYSLOG reception
#$ModLoad imtcp
#$InputTCPServerRun 514
   
Now create a haproxy.conf in /etc/rsyslog.d/ folder to create log files
vi /etc/rsyslog.d/haproxy.conf

Add the below line to create the file
local.* /var/log/haproxy.log
   
Restart the rsyslog services to update the changes we made
# service rsyslog restart
   

