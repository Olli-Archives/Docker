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

* docker run <-flag> <repository:tag>   This command is used to run a container from an image.  If you run docker images, it will give you the repository and tag for all of your images.  If you do not add a flag, the process will hang (terminal will run process and not allow another terminal command).  To avoid this you can run with a flag of d (detached).  This allows you to keep the docker container running while still running other commands.  Else you would need to control C which would kill the contianer.


* docker container ls | docker ps	This command is used to see all running containers 
