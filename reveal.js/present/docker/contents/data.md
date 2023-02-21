## Containerized Applications


<!-- .slide: data-transition="fade" -->
## Docker

https://www.docker.com/


<!-- .slide: data-transition="fade" -->
![Docker Internals](/present/docker/assets/docker-internals.png "Docker architecture")


<!-- .slide: data-transition="fade" -->
## Understanding Docker
A Docker container is a `box`: it has its own machine name and IP address.
it also has its own `disk drive` created by Docker.

Many containers can be run in parallel inside one computer without knowing one another

    docker container run IMAGE


<!-- .slide: data-transition="fade" -->
![Docker Containers](/present/docker/assets/docker-containers.png "Docker containers")


<!-- .slide: data-transition="fade" -->
## Pro Tip
<!-- .element: class="fragment" data-fragment-index="1" -->
Containers are running `only` while the application inside the container is running.
<!-- .element: class="fragment" data-fragment-index="2" -->
Containers in the exited state still exist, which means you can start them again, check the logs, and copy files to and from the container’s filesystem
<!-- .element: class="fragment" data-fragment-index="3" -->



<!-- .slide: data-transition="fade" -->
## Docker repository


<!-- .slide: data-transition="fade" -->
## Docker Hub
https://hub.docker.com/


<!-- .slide: data-transition="fade" -->
## Web ping example
A `Docker image` is logically one thing--you can think of it as a big zip file that contains the whole application stack.

    docker image pull diamol/ch03-web-ping

A Docker image is physically stored as lots of small files, called image layers and Docker assembles them together to create the containers filesystem.


<!-- .slide: data-transition="fade" -->
![Image Downloading](/present/docker/assets/downloading-image.png "Downloading Image")


Runs docker image

    docker container run diamol/ch03-web-ping

-it means interactively

    docker run -it diamol/ch03-web-ping

-d means detach mode

    docker run -d diamol/ch03-web-ping