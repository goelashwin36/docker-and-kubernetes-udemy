# Docker and Kubernetes


- Docker makes it really easy to install/run any software or application on any system or cloud computing environment without worrying aobut dependencies and configurations
- Docker image: A single file containging all the dependencies and configurations required to run the program
- Docker Container: Insance of an image that runs the program
-
- Container is an instance of an image


## Docker Ecosystem:

1. Docker Client
2. Docker Server
3. Docker Machine
4. Docker Images
5. Docker Hub
6. Docker
7. Compose


- Docker Daemon: Tool responsible for creating images, running instances(containers) etc.

- Installation: [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

```
╭─ash@ash ~
╰─$ docker version
Client: Docker Engine - Community
 Version:           19.03.12
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        48a66213fe
 Built:             Mon Jun 22 15:45:47 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.12
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       48a66213fe
  Built:            Mon Jun 22 15:44:17 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

╭─ash@ash ~
╰─$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:4cf9c47f86df71d48364001ede3a4fcd85ae80ce02ebad74156906caff5378bc
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/


```
- What happens when we run `docker run hello-world`
  1. Docker Client identifies that you want to run a new container with the name `hello-world`
  2. The client issued a command to the Docker Server.
  3. The Docker Server first checks if ther's a local copy of the image present or not.
  4. If not then it searched Docker Hub and fetches the image.
  5. Finally, it creates an instance of the image(container)


- Docker containers uses concepts like namespaces(for isolating resources per process or  of processes) and control groups(for limiting the amount of resources used per process) which are limited to Linux. So you must be wondering how Docker works in Windows and Mac OS right?

So behind the scenes, Docker uses a Linux Virtual Machine inside which all the containers are created. And the linux kernel is responsible for restricting access, managing containers. If you run the command `docker version`, you'll see that the OS mentioned is `linux/amd64` even if yours in windows or MacOS. This is because of the same reason.

- Based on the above discussion, consider container as a process or set of processes that have a grouping of resources specifically assigned to it.

- An image simply consists of a File System and a startup command
- A container consists of a running process, that has a specific set of resources assigned to it. The FS from the image is copied and put in the Hard Drive memory assigned to it.


## Commands

- `docker run <image_name>`

Run a Docker Container

```
╭─ash@ash ~
╰─$ sudo docker run busybox

```

- `docker run <image_name> <overriding_default_command>`

Run a Docker Container and override the default command
```
╭─ash@ash ~
╰─$ sudo docker run busybox echo hello                                    130 ↵
hello
```

- `docker ps`

List all running Containers

```
╭─ash@ash ~
╰─$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
851b38891150        busybox             "sh"                3 seconds ago       Up 3 seconds                            serene_taussig

```

- `docker ps -a`

List all the Containers

```
╭─ash@ash ~
╰─$ docker ps -a
CONTAINER ID        IMAGE                        COMMAND                  CREATED             STATUS                      PORTS               NAMES
851b38891150        busybox                      "sh"                     5 minutes ago       Exited (0) 11 seconds ago                       serene_taussig
14cff1f78e17        busybox                      "sh"                     8 minutes ago       Exited (0) 8 minutes ago                        jolly_merkle
6aab5ee7f6a1        busybox                      "sh"                     10 minutes ago      Exited (0) 10 minutes ago                       nice_merkle
0eb7b7f1a846        busybox                      "exec -it"               11 minutes ago      Created                                         cranky_ptolemy
46c8b84ff00a        busybox                      "-it"                    12 minutes ago      Created                                         kind_bhabha
e97a850fcdc2        hello-world                  "-it"                    12 minutes ago

and so on...
```

## Container Lifecycle

docker **run** = docker **create** + docker **start**

- Creating a container: `docker create <image_name>`

Here, only the file system is copied from the image to the hard drive segment for this container.

- Starting a container: `docker start -a <continer_id>`

Using this, the startup command of the container is executed

The `-a` flag tells the continer to eatch for output from the container and print it at the same time

- Restarting stopped containers: `docker start -a <previous_container_id>`

```
╭─ash@ash ~
╰─$ docker run busybox echo hello
hello
╭─ash@ash ~
╰─$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
3d52c4883562        busybox             "echo hello"        4 seconds ago       Exited (0) 3 seconds ago                       stoic_lalande
14cff1f78e17        busybox             "sh"                35 minutes ago      Up 12 minutes                                  jolly_merkle
╭─ash@ash ~
╰─$ docker start -a 3d52c4883562
hello
```

- Deleting all stopped containers: `docker system prune`

```
╭─ash@ash ~
╰─$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
3d52c4883562c2725502a3636ed54a998bcf46a2c3e0ff0605baf894811ecbea

Total reclaimed space: 0B
```

- Getting logs from a Container: `docker logs <container_id>`

Prints all the logs that were generated while running the container

```
╭─ash@ash ~
╰─$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
ccf3c4243b07        busybox             "echo hello"        7 seconds ago       Exited (0) 5 seconds ago                       boring_mendel
14cff1f78e17        busybox             "sh"                38 minutes ago      Up 15 minutes                                  jolly_merkle
╭─ash@ash ~
╰─$ docker logs ccf3c4243b07
hello

```

- Stop running containers

1. `docker stop <container_id>`

2. `docker kill <container_id>`