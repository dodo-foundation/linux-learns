**Docker Swarm**

Docker swarm is container orchestration build by Docker.Docker swarm converts multiple docker instances to singel virtual hosts.Docker swarm cluster consists generally contain three items
1) nodes
2) services and taks
3) Load Balancers
Nodes are individual instances of the Docker engine that control your cluster and manage the containers used to run your services and tasks. Docker Swarm clusters also include load balancing to route requests across nodes.

 **Advantages of Dockers Swarm**
 
Docker Swam is straightforward to install, especially for those just jumping into the container orchestration world. It is lightweight and easy to use. Also, Docker Swarm takes less time to understand than more complex orchestration tools. It provides automated load balancing within the Docker containers, whereas other container orchestration tools require manual efforts.

Docker Swarm works with the Docker CLI, so there is no need to run or install the entire new CLI. Plus, it works seamlessly with existing Docker tools such as Docker Compose.

Docker Swarm does not require configuration changes if your system is already running inside Docker.

**Disadvantages of Docker Swarm**

Despite its benefits, there are a few disadvantages of using Docker Swarm that you should be aware of.

First, it is lightweight and tied to the Docker API, which limits functionality in Docker Swarm, compared to Kubernetes. Likewise, Docker Swarm’s automation capabilities are not as robust as those offered by Kubernetes.

 **Kubernetes**  

Kubernetes is an open source container orchestration platform that was initially designed by Google to manage their containers. Kubernetes has a more complex cluster structure than Docker Swarm. It usually has a builder and worker nodes architecture divided further into pods, namespaces, config maps, and so on.

**Advantages Of Kubernetes**

Kubernetes offers a wide range of benefits to teams looking for a robust container orchestration tool:

It has a large open-source community, and Google backs it.
It supports every operating system.
It can sustain and manage large architectures and complex workloads.
It is automated and has a self-healing capacity that supports automatic scaling.
It has built-in monitoring and a wide range of available integrations.
It is offered by all three key cloud providers: Google, Azure, and AWS.
Because of its broad community support and ability to handle even the most complex deployment scenarios, Kubernetes is often the number one choice for enterprise development teams managing microservice-based applications.

**Disadvantages of Kubernetes**
Despite its comprehensive feature set, Kubernetes also has a few drawbacks:

It has a complex installation process and a steep learning curve.
It requires you to install separate CLI tools and learn each of them.
The transition from Docker Swarm to Kubernetes can be complicated and difficult to manage.
In some situations, Kubernetes can be overly complicated and lead to a loss of productivity.