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
view current images on computer
* docker images   This command is used to display all current images
create container from image
* docker run <-flag repository:tag>   This command is used to run a container from an image.  If you run docker images, it will give you the repository and tag for all of your images.  If you do not add a flag, the process will hang (terminal will run process and not allow another terminal command).  To avoid this you can run with a flag of d (detached).  This allows you to keep the docker container running while still running other commands.  Else you would need to control C which would kill the contianer.
Docker run will create a new container even if you stop it.  In order to delete old containers refer to section "Managing Containers below"

View current contaienrs
* docker container ls | docker ps	This command is used to see all running containers 

## Mapping Ports
-p flag can be used to map ports for example

* docker run -d -p 8080:80 nginx:latest
This will map local host port 8080 to port 80 on docker.  Port 80 on docker is a TCP port.
## Mapping multiple ports to single container
* docker run -d -p 3000:80 -p 8080:80 nginx:latest

## Managing Containers
Run docker container --help for additional info
Stop a container
* docker stop <name | ID>
Start a container bu using name or id
* docker start <name | ID>
to see running docker conatiners use
see currently running containers
* docker ps
remove a container 
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

## volumes
Volumes allow sharing of data.  This is helpful when sharing data between host and container and containers themselves.
For example an NGINX container hosts a volume. The volume allows sharing of data weteen the host (computer) and container (NGINX).
So if we create a document in the host computer, this document will also appear in the NGINX container. Vice versa, if you add a file to NGINX container, it will also appear on the host.

## creating a volume
* create a file that you want to become a volume.
* use the tag -v <path of volume you want to use:path on container where you want to copy volume to>
* Example of this woudl be docker run -d -p8080:80 -v $(pwd):/usr/share/nginx/html:ro nginx
In the example above, the docker hub describes where one should place a volume to on NGINX. :ro means read only.

## Modify files inside container
To do this we need to run docker exec.  this runs a command on a running docker container.
* docker exec -it (name or id of container) bash
the -it flag mean interactive mode with sudo.  Now you are in the docker and can cd into files.  Changing things within the volume will also change things on host

## Sharing Volumes Between Containers
Containers running on different ports can share files between the two.  this can be accomplished with a command named --volumes-from.  To see all possibilities that you can do 
withdocker run type docker run --help
* docker run --name "name-of-container" --volumes-from "name-of-container-that-has-volume" -d -p 80801:80 ngin
## Docker File
Docker files are instructions on how to create images
* https://docs.docker.com/engine/reference/builder/
Docker files always start with FROM.  Since we are using NGINX as base we can say
* FROM NGINX:latest

Add can be used to add something to the image.  We will copy everyting in the folder that contains the docker
file and place it to the folder where nginx expects to have all of its html
* ADD . /usr/share/nginx/html

WORKDIR means that if the container has a folder named the argument then use it else create it.  Any commands that follow WORKDIR will be excecuted in the defined container

* WORKDIR <./folder name>

RUN command is used to excece commands for example the command below will 
install all dependencies from package.json

* RUN npm i

CMD is used to run commands

* CMD node index.js


## Creating Images
docker build is used to create image from dockerfile.  The argument . means that the build will look for a docker file within the directory that tehe build script is called in.  You could give it an alternative path if needed
* docker build --tag website:latest .

## Tags, Versioning and Tagging

allows youto controll image version this avoids breaking changes.
If you run docker pull node:alpine you may bet different versions depending on when you pull the image from docker hub.   This may be a breaking changes.
You can find versions in docker hub under title of supported tags.

* in Docker file use FROM nginx:1.17.2-alpine.  This will force version to allways be the same

Tagging overide: When you build an image with :latest tag, it overrides the previous image and the prefious image will end up with a tag of none

* docker build -t website:latest .
* docker tag website:latest website:1

This will make another tag that is now versioned from latest 

## Docker Registries
Server that lets you store and distribute images.  Docker registries can be used in your CD/CI pipeline.  Ii is also used to run applications.  You can have private and public registries. 

To push images to docker hub follow these steps
* create a repository in Docker Hub
* once you create a repository it will give you a docker command to push to your repo images
* log in to docker
* docker push website/website:version (look at repo and this will make sense.  you may need to rename image to follow the naming pattern)

## Docker inspect
This is used to debug docker container.  This will give you info about the container which includes full state, if its running, host config, etc..
* docker inspect "name or id of container"

## Docker Logs
If you console log anything in a docker container, you can find it with the following

* docker logs "container id or name"
* docker logs -f "container id or name" will keep log alive in watch mode

## Docker Compose
When you have may containers using docker build and docker run to get
each container running can be tedious.  This is where docker compose comes in.

* Docker needs a docker compose file that is a yml file
  * version: '4' docker changes how compose files need written.  this specifies the verion you plan on using
  * services: used to specify services used
    * services:
      * any name:
        * build: ./path to image??
        * if building from image in docker hub you can just use image:image name
        * volumes: this is handled as a list ( list items start with -)
        * ports: handled as a list (list items start with -)

## Running Docker Compose
when you are in the same directory as the docker compose file, run
* docker compose up




## References
This writeup was made from reference to https://www.youtube.com/watch?v=jzbQt2MGf14&t=2421s. This guy is awesome and definitely worth a listen!
 