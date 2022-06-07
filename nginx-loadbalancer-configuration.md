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


Once you've created the Nginx image, execute it to create two containers with different ports, as seen below.

```bash

docker run -d -p 8081:80 --name Nginx1 imageid
docker run -d -p 8082:80 --name Nginx2 imageid

```



```bash
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
