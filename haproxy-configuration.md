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

global
	log /dev/log	local0
	log /dev/log	local1 notice
	chroot /var/lib/haproxy
	stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
	stats timeout 30s
	user haproxy
	group haproxy
	daemon

	# Default SSL material locations
	ca-base /etc/ssl/certs
	crt-base /etc/ssl/private

	# See: https://ssl-config.mozilla.org/#server=haproxy&server-version=2.0.3&config=intermediate
        ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets

defaults
	log	global
	mode	http
	option	httplog
	option	dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
	errorfile 400 /etc/haproxy/errors/400.http
	errorfile 403 /etc/haproxy/errors/403.http
	errorfile 408 /etc/haproxy/errors/408.http
	errorfile 500 /etc/haproxy/errors/500.http
	errorfile 502 /etc/haproxy/errors/502.http
	errorfile 503 /etc/haproxy/errors/503.http
	errorfile 504 /etc/haproxy/errors/504.http


frontend http_front
   bind *:6443
   stats uri /haproxy?stats
   default_backend http_back

backend http_back
   balance roundrobin
   server s1 192.168.0.105:8081 check
   server s2 192.168.0.109:8081 check

``` 

