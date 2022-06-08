## nginx loadbalancer configuration.md


### Load Balancer Process


 Load balancing is the process of distributing network traffic across multiple servers. This ensures no single server bears too much demand. By spreading the work evenly, load balancing improves application responsiveness. It also increases availability of applications and websites for users.


![Screenshot from 2022-05-30 18-33-53](https://user-images.githubusercontent.com/102893121/170998449-9d31773c-6f59-414d-9db8-5a3c98340c67.png)

**_configuration_**

In this section 2 Nginx container running as a server
  * Nginx1
  * Nginx2

_create Nginx DockerFile_   

**Note:** To create an Nginx container, go to this post.


https://github.com/dodo-foundation/linux-learns/blob/main/dockercustom_nginx.md


Once you've created the Nginx image, Run it to create two containers with different ports, as seen below.

```bash

docker run -d -p 8081:80 --name Nginx1 imageid
docker run -d -p 8082:80 --name Nginx2 imageid

```


```bash
# /etc/nginx/sites-available/fourtimes.ml.conf
upstream backend{
        server localhost:8081;
        server localhost:8082;
}
server {
        listen 80;
        location / {
           proxy_pass http://backend;
        }
}
```

**_configuration with SSL_**

In this task we are take config loadbalancer with SSL certificate 


```bash
# /etc/nginx/sites-available/fourtimes.ml.conf
upstream backend{
        server localhost:8081;
        server localhost:8082;
}


server {
        listen 80;
        server_name fourtimes.ml;
        
	#access_log           /var/log/nginx/access.log;
        #error_log            /var/log/nginx/error.log;
	return 301 https://$host$request_uri;
        
	location / {
           proxy_pass http://backend;
        }
}


server {
    listen                443 ssl;
    server_name fourtimes.ml;

    access_log           /var/log/nginx/access.log;
    error_log            /var/log/nginx/error.log;

    location / {
	    proxy_pass http://backend;
    }

    ssl                  on;
    ssl_certificate      /etc/nginx/ssl-certificate/certificate.crt;
    ssl_certificate_key  /etc/nginx/ssl-certificate/private.key;

    }


```

_**output**_

```bash

curl localost:8081
curl localhost:8082

```

 * Note --> Try your web browser `yourip:8081`
