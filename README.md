# docker
My docker configurations and cheatsheet.

> Written with ***Typora***: https://www.typora.io/
>
> More or less a partial summary of the [official docker docs](https://docs.docker.com/)



## Official Cheatsheet

> https://www.docker.com/sites/default/files/Docker_CheatSheet_08.09.2016_0.pdf



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



* `docker build -t example.` 
  Create Docker image and tag with custom name (according to Dockerfile)

* `docker run -p 4000:80 example`

  * Run app, mapping your machine’s port 4000 to the container’s published port 80 using `-p`

  * Now you can visit `http://localhost:4000

* `CTRL + C`

  * Stop app if running
  * On Windows systems: first type `CTRL+C` to get the prompt back, then type  `docker container ls`  to list the running containers, followed by `docker container stop <Container NAME or ID>`  to stop the container

* `docker run -d -p 4000:80 example` 

  Run app in the background (detached mode). You get long container ID returned.

* `docker container stop` + containerID
  Stop container with given container ID

## Glossary

* **Dockerfile**: *A file that defines what goes on in the environment inside your container*. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world, and be specific about what files you want to “copy in” to that environment. However, after doing that, you can expect that the build of your app defined in this `Dockerfile` behaves exactly the same wherever it runs.
* 

## Tipps

* To avoid permission errors (and the use of `sudo`), add your user to the `docker` group. [Read more](https://docs.docker.com/engine/installation/linux/linux-postinstall/).

* Accessing the name of the host when inside a container retrieves the container ID, which is like the process ID for a running executable.
* If you are using Docker Toolbox on Windows 7, use the Docker Machine IP instead of `localhost`. For example, http://192.168.99.100:4000/. To find the IP address, use the command `docker-machine ip`.
* The long and short containerID both work interchangeably



