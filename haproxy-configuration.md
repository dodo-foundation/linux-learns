## haproxy configuration

haproxy configuration using the purpose for load balancing and high availability and proxy functionality. haproxy is particularly suited for very high traffic websites and is therefore often used to improve web service reliability and performance for multi-server configurations

_**process flow**_

![image](https://assets.digitalocean.com/articles/high_availability/ha-diagram-animated.gif)


**Examination of requiremnets**

|SERVER| IPADDRESS|OS|
|---|---|---|
|haproxy-a| 192.168.0.2| ubuntu |
|haproxy-b| 192.168.0.3| ubuntu |

**server installtion**

```bash

sudo apt update 
sudo apt install -y haproxy

```

_**HAProxy validation**_

```bash

haproxy -v

```

**Configuraton**

Configuring Load balancer on Layer 1

```bash
# sudo vi /etc/haproxy/haproxy.cfg


frontend http_front
   bind *:6443
   stats uri /haproxy?stats
   default_backend http_back

backend http_back
   balance roundrobin
   server <server1 name> <private IP 1>:80 check
   server <server2 name> <private IP 2>:80 check
``` 

