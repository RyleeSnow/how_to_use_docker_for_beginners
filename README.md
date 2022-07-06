# How to use docker for the first time

**System: Linux Ubuntu** 
<br>
<br>

## "Hello World"
Usually the docker engine and docker compose have been installed if you are using a server provided by Azure or other cloud platforms.

Check the docker version:
```bash
$ docker --version  # check the docker engine version
$ docker-compose --version  # check the docker compose version
```

If the version can meet your requirement, you can try to run the "Hello-World" image.
```bash
$ sudo service docker start 
$ docker run hello-world
```

If the docker is working well, you shall see some outputs like:

_"Hello from Docker! This message shows that your installation appears to be working correctly."_
<br>
<br>

## Install Docker
Instead, if the docker hasn't been installed or the version doesn't match, you can install docker by yourself.

[reference: https://docs.docker.com/engine/install/ubuntu/]

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc

$ sudo apt-get update

$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
$ sudo mkdir -p /etc/apt/keyrings

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$ sudo apt-get update

$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
 
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

- Sometimes, you may need to install a specific version of docker-composer (version 1.26 as the example).
```bash
$ curl -L https://github.com/docker/compose/releases/download/1.26.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
```
<br>
<br>

## Install Image

If you have the local image files, including:
- environment_setting (.py)
- docker-compose (.yml)
- Dockerfile

BUild the image:
```bash
$ python environment_setting.py [options]
```

You can use the following command to check if the docker image is correctly installed. You shall see the image name and **image_ID**.
```bash
$ docker images
```
<br>
<br>

## Create and Run a Container

Create a container based on the image:
```bash
$ docker run -it {image_ID} /bin/bash
```

After you entering the container, you can see the container_ID as **root@{container_ID}**
If you want to use the conda environment, you need to do:
```bash
$ conda init bash

$ exit

$ docker start {container_ID}

$ docker attach {container_ID}

$ conda activate {conda_environment_name}
```

Anytime you want you check your containers:
```bash
$ docker ps -a
```
<br>
<br>

## Transfer files between the host and the container

Get the container_long_ID:
```bash
$ docker inspect -f '{{.Id}}' {container_ID}
```

Transfer files:
```bash
$ docker cp {your_host_file_path} {container_long_ID}:{the_container_file_path}
```
<br>
<br>

## Manage Images and Containers
Manage images:
```bash
$ docker images  # check image
$ docker run {image_ID}  # run image

$ docker rmi -f {image_ID}  # delete a single image
$ docker rmi -f {image_ID_1} {image_ID_2} {image_ID_3}  # delete multiple images
$ docker rmi -f $(docker images -aq)  # delete all images
```
Manage containers:
```bash
$ docker ps  # check containers

$ docker start {container_ID}  # start a container
$ docker restart {container_ID}  # restart a container
$ docker stop {container_ID}  # stop a container
$ docker kill {container_ID}  # stop the current container

$ docker exec -it {container_ID} /bin/bash  # enter a container and start a new terminal
$ docker attach {container_ID}  # enter a container and use the current terminal

docker rm -f {container_ID}  # delete a single container
docker rm -f $(docker ps -aq)  # delete all containers
```
