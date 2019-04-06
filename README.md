# docker
My docker configurations, tipps and cheatsheet.

> Written with ***Typora***: https://www.typora.io/
>
> More or less a partial summary of the [official docker docs](https://docs.docker.com/)
>
> If you don't have a docker account, sign up for one at [hub.docker.com](https://hub.docker.com/).



## Official Cheatsheet

> https://www.docker.com/sites/default/files/Docker_CheatSheet_08.09.2016_0.pdf



```shell
# Create a VM (Mac, Win7, Linux)
docker-machine create --driver virtualbox myvm1 

# Win10
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1

# View basic information about your node
docker-machine env myvm1             

# List the nodes in your swarm
docker-machine ssh myvm1 "docker node ls"         

# Inspect a node
docker-machine ssh myvm1 "docker node inspect <node ID>"       

# View join token
docker-machine ssh myvm1 "docker swarm join-token -q worker" 

# Open an SSH session with the VM; type "exit" to end
docker-machine ssh myvm1   

# View nodes in swarm (while logged on to manager)
docker node ls        

# Make the worker leave the swarm
docker-machine ssh myvm2 "docker swarm leave"  

# Make master leave, kill swarm
docker-machine ssh myvm1 "docker swarm leave -f" 

# list VMs, asterisk shows which VM this shell is talking to
docker-machine ls 

# Start a VM that is currently not running
docker-machine start myvm1     

# show environment variables and command for myvm1
docker-machine env myvm1      

# Mac command to connect shell to myvm1
eval $(docker-machine env myvm1)    

# Windows command to connect shell to myvm1
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression   

# Deploy an app; command shell must be set to talk to manager (myvm1), uses local Compose file
docker stack deploy -c <file> <app>  

# Copy file to node's home dir (only required if you use ssh to connect to manager and deploy the app)
docker-machine scp docker-compose.yml myvm1:~ 

# Deploy an app using ssh (you must have first copied the Compose file to myvm1)
docker-machine ssh myvm1 "docker stack deploy -c <file> <app>" 

# Disconnect shell from VMs, use native docker
eval $(docker-machine env -u)     

# Stop all running VMs
docker-machine stop $(docker-machine ls -q)     

# Delete all VMs and their disk images
docker-machine rm $(docker-machine ls -q) 
```



## Installation

* Mac: https://docs.docker.com/docker-for-mac/install/
* Windows: https://docs.docker.com/docker-for-windows/install/
* Centos: https://docs.docker.com/install/linux/docker-ce/centos/
* Debian: https://docs.docker.com/install/linux/docker-ce/debian/
* Fedora: https://docs.docker.com/install/linux/docker-ce/fedora/
* Ubuntu: https://docs.docker.com/install/linux/docker-ce/ubuntu/



## Basics

* `docker version` 
  Show installed Docker version
* `docker info` 
  Show more details about your Docker installation

* `docker run hello-world` 
  Execute Docker image "hello-world"

* `docker image ls` 
  List Docker images



* `docker ps` 
  List running containers
* `docker container ls` 
  List running containers
* `docker container ls --all` 
  List all Docker containers
* `docker container ls -aq` 
  List all Docker containers in quiet mode
* `docker container rm $(docker container ls -a -q)`
  Remove all containers
* `docker container rm <hash>`
  Remove specified container from your machine



* `docker build -t example.` 
  Create Docker image and tag with custom name (according to Dockerfile)

* `docker run -p 4000:80 example`

  * Run app, mapping your machine’s port 4000 to the container’s published port 80 using `-p`

  * Now you can visit `http://localhost:4000

* `CTRL + C`

  * Stop app if running
  * On Windows systems: first type `CTRL + C` to get the prompt back, then type  `docker container ls`  to list the running containers, followed by `docker container stop <hash>`  to stop the container

* `docker run -d -p 4000:80 example` 

  Run app in the background (detached mode). You get long container ID returned.

* `docker container stop <container id>`
  Stop container with given container ID

* `docker container kill <hash>`
  Force shutdown of the specified container



* `docker image ls -a` 
  List all image on this machine
* `docker image rm <image id>` 
  Remove specified image from this machine

* `docker image rm $(docker image ls -a -q)` 
  Remove all images from this machine



* `docker login` 
  Log in to the Docker public registry on your local machine
* `docker tag <image name> <username>/<repository name>:<tag>` 
  Associate a local image with a repository on a registry (collection of repositories). Uploads the image to that repository.
* `docker push <username>/<repository name>:<tag>` 
  Upload tagged image to the repository. Once complete, the results of this upload are publicly available. If you log in to [Docker Hub](https://hub.docker.com/), you see the new image there, with its pull command (or https://cloud.docker.com/).
* `docker run -p 4000:80 <username>/<repository name>:<tag>` 
  Run image from a registry.



## Run load-balanced app

> A *load balancer* serves as the single point of contact for clients. The load balancer distributes incoming application traffic across multiple targets, such as EC2 instances (Amazon), in multiple Availability Zones. This increases the availability of your application. You add one or more listeners to your load balancer.



* `docker swarm init` 
  In directory where your docker-compose.yml file is, you have to run this command to initialize a swarm and turn the node into a swarm manager.
* `docker swarm join`
  Run on other machines to have them join the swarm as workers.
* `docker stack deploy -c <compose-file> <app-name-here>` 
  Runs app, references the docker-compose.yml file to create images according to it and assigns a name to the app. This can also be executed to apply changes to the service.
* `docker stack ps <app-name-here>`
  List all apps in stack
* `docker service ls` 
  List all services.
* `docker service ps <app-name-here>`
  List all tasks of given application.
* `docker stack rm <app-name-here>`
  Take down the given application.
* `docker swarm leave --force` 
  Take down the swarm.
* `docker inspect <task or container>`
  Inspect task or container.



## Clusters (& VMs)

* `docker-machine create --driver virtualbox <name>`
  Create a virtual machine with docker
* `docker-machine ls`
  List all virtual machines
* `docker-machine ssh <virtual-machine> "<command>"`
  Send commands to virtual-machine
* `docker-machine ssh <virtual-machine> "docker swarm init --advertise-addr <virtual-machine's ip>"`
  Instruct my virtual machine to become a swarm manager
* `docker-machine env <machine>`
  To get and run a command that configures your current shell to talk to the Docker daemon on the VM
* https://docs.docker.com/get-started/part4/#configure-a-docker-machine-shell-to-the-swarm-manager
  Configure a `docker-machine` shell to the swarm manager
* `docker-machine ssh <virtual-machine> "docker swarm leave --force"`
  Remove the swarm manager on given virtual machine
* `docker-machine ssh <virtual-machine> "docker swarm leave"`
  Remove the swarm worker on given virtual machine

* `eval $(docker-machine env -u)`
  Unset environment variables (Mac)
* `docker-machine stop $(docker-machine ls -q)`
  Stop all running VMs


## Glossary

* **Dockerfile**: *A file that defines what goes on in the environment inside your container*. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world, and be specific about what files you want to “copy in” to that environment. However, after doing that, you can expect that the build of your app defined in this `Dockerfile` behaves exactly the same wherever it runs.
* **Registry**: *collection of repositories, and a repository is a collection of images*—sort of like a GitHub repository, except the code is already built. An account on a registry can create many repositories. The `docker` CLI uses Docker’s public registry by default.
* **Task**: *A single container running in a service is called a task*. Tasks are given unique IDs that numerically increment, up to the number of `replicas` you defined in `docker-compose.yml`
* **Swarm**: Multi-container, multi-machine applications are made possible by joining multiple machines into a “Dockerized” cluster called a **swarm**. A swarm is made up of multiple nodes, which can be either physical or virtual machines. 
* **Swarm Manager**: *Manager of your swarms*. Can use several strategies to run containers, such as “emptiest node” -- which fills the least utilized machines with containers. Are the only machines in a swarm that can execute your commands, or authorize other machines to join the swarm as workers.
* **Stack**: *A group of interrelated services that share dependencies, and can be orchestrated and scaled together*. A single stack is capable of defining and coordinating the functionality of an entire application (though very complex applications may want to use multiple stacks).



## Tipps

* To avoid permission errors (and the use of `sudo`), add your user to the `docker` group. [Read more](https://docs.docker.com/engine/installation/linux/linux-postinstall/).
* Accessing the name of the host when inside a container retrieves the container ID, which is like the process ID for a running executable.
* If you are using Docker Toolbox on Windows 7, use the Docker Machine IP instead of `localhost`. For example, http://192.168.99.100:4000/. To find the IP address, use the command `docker-machine ip`.
* The long and short containerID both work interchangeably
* To be able to create clusters you need to download and install [**Oracle VirtualBox**](https://www.virtualbox.org/wiki/Downloads)
* 



