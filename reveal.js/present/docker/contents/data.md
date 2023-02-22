## Containerized Applications


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

`-it` means interactively

    docker run -it diamol/ch03-web-ping

`-d` means detach mode

    docker run -d diamol/ch03-web-ping


<!-- .slide: data-transition="fade" -->
## Passing ENV variables
Docker images may be packaged with a default set of configuration values for the application, 
but you should be able to provide different configuration settings when you run a container.

    docker container run --env TARGET=google.com diamol/ch03-web-ping


<!-- .slide: data-transition="fade" -->
## Building Docker Images
The `Dockerfile` is a simple script you write to package up an application.
it’s a set of instructions, and a Docker image is the `output`.
```dockerfile
FROM diamol/node

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```


<!-- .slide: data-transition="fade" -->
Base your image from pre-configured environments
In this case with node already installed.
```dockerfile [1]
FROM diamol/node

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```


<!-- .slide: data-transition="fade" -->
You can configure a set of environment variables.
```dockerfile [3-5]
FROM diamol/node

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```


<!-- .slide: data-transition="fade" -->
`WORKDIR` specifies the current directory from the image to work with.
We create the directory `web-ping` at the root of the image file directory.
```dockerfile [7]
FROM diamol/node

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```


<!-- .slide: data-transition="fade" -->
`COPY` Copies the file `app.js` from your current working directory
to the directory of the image, in this case `/web-ping`

Download code for [app.js](https://github.com/sixeyed/diamol/blob/master/ch03/exercises/web-ping/app.js)
```dockerfile [8]
FROM diamol/node

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```


<!-- .slide: data-transition="fade" -->
`CMD` Specifies the launch command used when we run the container.

**When we run: `docker run MY_IMAGE`
```dockerfile [10]
FROM diamol/node

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```


<!-- .slide: data-transition="fade" -->
Build docker image with

    docker image build --tag web-ping


<!-- .slide: data-transition="fade" -->
[Most used commands](/present/docker/contents/commands.md)