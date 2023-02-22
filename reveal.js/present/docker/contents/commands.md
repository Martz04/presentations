
Starts a new container

    Docker container run IMAGE_NAME

List active containers

    docker container ls

List inactive containers

    docker container ls -a

Logs from a container
    
    docker container logs CONTAINER_ID

Information about the container

    docker container inspect CONTAINER_ID

Run a container in interactive mode

    docker container run --interactive --tty IMAGE

Resource usage from a container

    docker container stats CONTAINER_ID

Turns off a currently active container
Still lives in your machine

    Docker container stop CONTAINER_ID

Deletes a container from your machine registry

    docker container rm --force CONTAINER_ID