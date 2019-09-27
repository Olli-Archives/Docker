# DOCKER

## Image 
* Image is a template for creating an environment of your choice
* Image is a snapshot (version of an image).  This means that if you deploy to production an image and it introduces a bug, you can just point to the previous version of the image
to make everything work again.
* Image contains everything that it needs to run (OS, Software, App code etc)

## Container
Container is a running instance of an Image

## Docker Hub
Docker Hub is a registry which houses images. You can for example download an NGINX img and spin it up within a container.  Docker Hub website is https://hub.docker.com

## Docker CLI Commands
* docker pull <name of image>   This command is used to downlaod image from docker container.  Each image should give you a name of image used for download

* docker images   This command is used to display all current images

* docker run <-flag repository:tag>   This command is used to run a container from an image.  If you run docker images, it will give you the repository and tag for all of your images.  If you do not add a flag, the process will hang (terminal will run process and not allow another terminal command).  To avoid this you can run with a flag of d (detached).  This allows you to keep the docker container running while still running other commands.  Else you would need to control C which would kill the contianer.
Docker run will create a new container even if you stop it.  In order to delete old containers refer to section "Managing Containers below"

* docker container ls | docker ps	This command is used to see all running containers 

## Mapping Ports
-p flag can be used to map ports for example
* docker run -d -p 8080:80 nginx:latest
This will map local host port 8080 to port 80 on docker.  Port 80 on docker is a TCP port.
## Mapping multiple ports to single container
* docker run -d -p 3000:80 -p 8080:80 nginx:latest

## Managing Containers
to stop a container you can use the following command
* docker stop <name | ID>
you can also start a container bu using name or id
* docker start <name | ID>
to see running docker conatiners use
* docker ps
docker ps also does alot of other things.  To see all capabilities use
docker ps --help
* docker rm is used to remove old docker files.  You can also use docker rm $(docker ps -aq) to delete all non active containers!!
The -aq flags gives only the serial number of the container back.

## Naming Containers
* docker run --name <enter name> -d -p8080:80 nginx:latest
naming is good since you will be running multiple containers at same time.  This makes things less confusing.

## Formating with global variables.
* you can make global variables using export variableName=xxxxx
* you can call for global variables sing the $variableName
* you can now call docker ps --format=$FORMAT
* it seems that you could also configure teh docker file in root directory to automate this even further

# volumes
Volumes allow sharing of data.  This is helpful when sharing data between host and container and containers themselves.
For example an NGINX container hosts a volume. The volume allows sharing of data weteen the host (computer) and container (NGINX).
So if we create a document in the host computer, this document will also appear in the NGINX container. Vice versa, if you add a file to NGINX container, it will also appear on the host.

## creating a volume
* create a file that you want to become a volume.
* use the tag -v <path of volume you want to use:path on container where you want to copy volume to>
* Example of this woudl be docker run -d -p8080:80 -v $(pwd):/usr/share/nginx/html:ro nginx
In the example above, the docker hub describes where one should place a volume to on NGINX 
