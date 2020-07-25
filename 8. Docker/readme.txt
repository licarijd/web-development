Docker 


Docker Containers

- before Docker, we had Virtual Machines

- Docker containers wrap up the software in a complete filesystem with everything it 
needs to run (code, runtime, system tools, system libraries, settings)

- anything you can install on a server, you can use with Docker 

With Docker container's you use the host's operating system, unlike VMs which 
have their own OS.

The goal is to have portable applications which run on any platform.


Docker is used to create Containers, and inside Containers we have an Image.

An image is what Docker uses to bundle your application into a standalone package 
that can live inside of a Container. 

The enironment inside a container is completely isolated from the host machine.


Docker Hub 

- provides a store-like webiste where you can get Images to use, eg. a Node Image 


** You usually won't write your own Dockerfiles!!


A database, Redis, and an API server could all be in a container, for example.

A container holds a micoroservice, does one thing very well (eg. the SonarQube container).


Docker Commands 


docker build -t <container name>


docker run -it -d <container>

- run, don't enter 


docker ps 

- list processes 


docker exec -it <container hash> bash

- enter into container which is already running 


docker stop <container hash>


More commands can be found here: https://docs.docker.com/engine/reference/commandline/docker/


Port Binding 

- binds a port on your computer to a port in the Docker container

docker run -it -p 3000:3000 <container name>


Docker Compose 

- launches multiple containers 

- orchestrates application services during development 


Once all containers are built, we can use: 

docker-compose up [-d]

to bring up all services defined in docker-compose.yml, unlike docker-compose run, which 
runs a single service. It also defines services running by their container name in the 
terminal.


Volumes 

- can be added to a service in docker-compose.yml 

- a way to mount what we have on our computer to our Docker container, so changes we make 
can be relected in the container (and viewed in the terminal)


docker-compose down 

- brings down all services (opposite of 'docker-compose up')


exec <service-name> bash 

- enter bash shell of a service (service must be up)



