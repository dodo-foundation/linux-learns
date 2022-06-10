## haproxy configuration

haproxy configuration using the purpose for load balancing and high availability and proxy functionality. haproxy is particularly suited for very high traffic websites and is therefore often used to improve web service reliability and performance for multi-server configurations

_**process flow**_

![image](https://assets.digitalocean.com/articles/high_availability/ha-diagram-animated.gif)


**Examination of requiremnets**

|SERVER| IPADDRESS|OS|
|---|---|---|
|Server-1| 192.168.0.105| ubuntu |
|Server -2| 192.168.0.109| ubuntu |
|Floating Ip| 192.168.0.110| ubuntu|

_**Pre-steps**_

connect the `each machine` and execute the host entry

```bash
echo "192.168.0.105 Server1" | sudo tee -a /etc/hosts
echo "192.168.0.109 Server2" | sudo tee -a /etc/hosts
echo "loalhost:8081  Virtual ip" | sudo tee -a /etc/hosts
```

**Nginx Container Creation**

In this case, the Nginx container serves as the server running on the '8081 port' and the link below is used to create the nginx container.

https://github.com/dodo-foundation/linux-learns/blob/5d7e943997f90e54b06caef8a450f56fd5461b8d/dockercustom_nginx.md

---

**Keepalived Creation**

We can try keepalived with ha-proxy method in this task, so you must install config in 'keepalived'. If Keepalived is already installed, skip this step. Otherwise, check out this link to install and configure Keepalived.

https://github.com/dodo-foundation/linux-learns/blob/b487d9a173e228b012b477328d9a93559a2d6357/keepalived-configuration.md

---

**HA-proxy installtion**

```bash

sudo apt update 
sudo apt install -y haproxy

```

_**HAProxy validation**_

```bash

haproxy -v

```

**Configuraton**

_**Configuring Load balancer on `Server 1**_

* In this secton conf file located in `/etc/haproxy/haproxy.cfg`

_backup_

```bash 

sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bk

```

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

_Check Configuration_

```bash

haproxy -c -V -f /etc/haproxy/haproxy.cfg

```

After running the command, the message 'Configuration file is valid' appears. 

```bash 
curl -i localhost:6443
curl -i 192.168.0.110:6443
```

to shown your `index.html` file is automatically changed


_**Configuring Load BAlancer on Server2**_

_Backup Orginal File_

```bash 

sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bk

```

```bash
# sudo vim /etc/haproxy/haproxy.cfg

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

_Check Configuration_

```bash

haproxy -c -V -f /etc/haproxy/haproxy.cfg

```

After running the command, the message appears 'Configuration file is valid'. 

```bash 
curl localhost:6443
curl 192.168.0.110:6443

#output
#Its works Container1
#Its works Container2
```

* In this case we check floating ip using with keepalived Conf
* Output Content from two different servers will appear alternately.

