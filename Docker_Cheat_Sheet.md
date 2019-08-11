Cheat Sheet for Docker Commands:
===============================

Testing Docker first after installation:

$ docker run hello-world

Running Docker (bydefault foregroud)

$ docker run busybox:1.30 echo "Hello World"

Checking Docker images locally:

$ docker images

Delete Docker Images

$ docker rmi -f tomcat 		## rmi=remove image  tomcat=image name


List out directory inside container busybox (Linux):

$ docker run busybox:1.30 ls /


Spinning up container with interactive mode (-i) & pseudo TTY (-t)

$ docker run -i -t busybox:1.30


Check current running container

$ docker ps

Check the containers run as of now.

$ docker ps -a

How to kill the running container?

$ docker ps -a 	# It will also display your running container (along with others) with "UP 10 (any time value) second", kill it.
$ dicker kill a1ef$2sd (a1ef$2sd = container_ID)


Running Container in background which will run for certain time.
Explanation: Without any process running on container, it does not stay in running state that why by using sleep 1000 (in sec)
			it enable container to stay in running state, now you will be able to "SSH" to busy box

$ docker run -d busybox:1.30 sleep 1000

How to spinup container with meaningful name:

$ docker run --name hello_world busybox:1.30


How to take information about running container specially networking information & others

$ docker inspect ca0a48251710118db6c797f6bdeb7cde11ca9d23743ed411d9410265b04151d2 # (container_id)


Running container for certain period, --rm meaning once you comeout from ineteractive shell
container will stop and image also remove, you will not find that image in the list of "docker images"

$ docker run -it --rm -p 8888:8080 tomcat:8.0


Docker port mapping with host and container:

$ docker run -it -d -p 8888:8080 tomcat:7.0 # 8888 for Host port | 8080 running for container port

Remove docker image             # Before delete image you must make sure that all dependent container has been shutdown

$ docker rm containet_Id


Running container on Foreground 

$ docker run kodekloud/simple-webapp

Running container on Background

$ docker run -d kodekloud/simple-webapp           # running container in background i.e "-d" = detached mode
  output: dnndsfhwfhfnwoewh983274932875rfndskjfy3
  
Again same container bringing into Foreground

docker attach dnndsfhw


Volume Mapping:

Container path = /var/lib/mysql    Host path = /opt/datadir

$ docker run -v /opt/datadir:/var/lib/mysql mysql:latest


* Assigning Resources to Docker Container -

$ docker run --cpu=.5 ubuntu		# .5 means 50% of cpus
$ docker run --memory=100m ubuntu

-----------------------
Building Docker Images:

$ docker run -it debian:jessie

root@56e2f301fbc4:/# yum install git

from another terminal build the image

Syntax: docker commit container_id repository_name:tag

$ docker commit 56e2f301fbc4 dasb/debian:1.00

now spin up new container:

$ docker run -it dasb/debian:1.00

## You build new images successfully and spinned up with new container
----------------------


How enter (loging) to the running container

$ docker exec -it <container_id> bash


How to stop container

docker stop <container_id> .. ..







Sample Dockerfile:


FROM debian:jessie
RUN apt-get update && apt-get install -y \
git \
vim


Build the image from Dockerfile.

$ docker build -t dasb/debian:1.01 .          # Note "." is my current directory here, if you want you can specify the path using -f flag



Pushing images to Repository:

$ docker login --username=dasbtutorial
Password:
Login Succeeded
$ docker push dasb/debian:1.01 <-|



Docker Networking:
=================

$ docker network ls      # You can see the bridge network initialized

$ docker network inspect bridge         # Check "Subnet:" line


Examples:

$ docker run -d --name container_1 busybox sleep 1000      # Make a note, we did not mention "--net" her
                                                             It is going to choose Bridge network automatically.
                                                             
$ docker exec -it container_1 ifconfig                     # You will see 2 Ethernet Interface 1) Loopback 2) Eth0 (Private)
                                                           # Container_1: Make a note of IP Address : 172.17.0.2
$ docket run -d --name container_2 busybox sleep 1000
$ docker exec -it container_1 ifconfig                     # Container_2: Make a note of IP Address : 172.17.0.3

Try Ping -

$ docker exec -it container_1 ping 172.17.0.3               # It must response

$ docker exec -it container_1 ping 8.8.8.8                  # Public network



## Creating Custom Bridge Network

$ docker network create --driver bridge my_bridge_network       # my_bridge_network is my custom name

$ docker network ls                                             # You will see newly created Bridge Network



>>> How to set root passowrd for Docker Container:

https://docs.docker.com/engine/examples/running_ssh_service/