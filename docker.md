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
