# Docker and Kubernetes

-   Docker makes it really easy to install/run any software or application on any system or cloud computing environment without worrying aobut dependencies and configurations
-   Docker image: A single file containging all the dependencies and configurations required to run the program
-   Docker Container: Insance of an image that runs the program
-
-   Container is an instance of an image

## Docker Ecosystem:

1. Docker Client
2. Docker Server
3. Docker Machine
4. Docker Images
5. Docker Hub
6. Docker
7. Compose

-   Docker Daemon: Tool responsible for creating images, running instances(containers) etc.

-   Installation: [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

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

-   What happens when we run `docker run hello-world`

    1. Docker Client identifies that you want to run a new container with the name `hello-world`
    2. The client issued a command to the Docker Server.
    3. The Docker Server first checks if ther's a local copy of the image present or not.
    4. If not then it searched Docker Hub and fetches the image.
    5. Finally, it creates an instance of the image(container)

-   Docker containers uses concepts like namespaces(for isolating resources per process or of processes) and control groups(for limiting the amount of resources used per process) which are limited to Linux. So you must be wondering how Docker works in Windows and Mac OS right?

So behind the scenes, Docker uses a Linux Virtual Machine inside which all the containers are created. And the linux kernel is responsible for restricting access, managing containers. If you run the command `docker version`, you'll see that the OS mentioned is `linux/amd64` even if yours in windows or MacOS. This is because of the same reason.

-   Based on the above discussion, consider container as a process or set of processes that have a grouping of resources specifically assigned to it.

-   An image simply consists of a File System and a startup command
-   A container consists of a running process, that has a specific set of resources assigned to it. The FS from the image is copied and put in the Hard Drive memory assigned to it.

## Commands

-   `docker run <image_name>`

Run a Docker Container

```
╭─ash@ash ~
╰─$ sudo docker run busybox

```

-   `docker run <image_name> <overriding_default_command>`

Run a Docker Container and override the default command

```
╭─ash@ash ~
╰─$ sudo docker run busybox echo hello                                    130 ↵
hello
```

-   `docker ps`

List all running Containers

```
╭─ash@ash ~
╰─$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
851b38891150        busybox             "sh"                3 seconds ago       Up 3 seconds                            serene_taussig

```

-   `docker ps -a`

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

-   Creating a container: `docker create <image_name>`

Here, only the file system is copied from the image to the hard drive segment for this container.

-   Starting a container: `docker start -a <continer_id>`

Using this, the startup command of the container is executed

The `-a` flag tells the continer to eatch for output from the container and print it at the same time

-   Restarting stopped containers: `docker start -a <previous_container_id>`

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

-   Deleting all stopped containers: `docker system prune`

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

-   Getting logs from a Container: `docker logs <container_id>`

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

-   Stop running containers

1. `docker stop <container_id>`

2. `docker kill <container_id>`

Both of them can be used to stop a container.

The difference being, `docker stop` uses `SIGTERM` signal which basically performs a bunch of tasks like cleanup, saving some files etc. and shuts down the container safely.
However, `docker kill` uses `SIGKILL` signal which shuts down the container immediately.

_**Note**: If the `docker stop` command is not able to stop the container, it automatically executes `kill` command after some time._

## Running commands in a running container

`docker exec <container_id> <command>` helps us to run a command in a running container

`-it` flag is a combination of 2 flags

1. `-i` tells the container to keep the STDIN open and it allows us to give inputs/commands
2. `-t` attaches a TTY(terminal) to the container and it prints everything nicely formatted

-   Launch a shell inside the container: `docker exec -it <container_id> sh`
    Here `sh` is basically a shell which allows us to execute commands. It could also be `bash`, `zsh` or `powershell`.

Till now we figured out how to launch shell in a running container. Now, to start directly with a shell:
`docker run -it <container_name> sh`

```bash
╭─ash@ash ~/Documents/Github/DockerAndKubernetes ‹master*›
╰─$ docker run -it ubuntu bash
root@84d1879ef1bf:/# echo hello
hello
```

## Building Custom Docker Images


To build custom docker images, we write a `Dockerfile` which has all the configurations to define the docker container and its behaviour

Workflow:

1. Specify a base image(Alpine, Ubuntu etc.)
2. Add the commands to be run on the base image(installing some programs, creaing directory, copy files etc.)
3. Specify a startup command(to be run when the container starts).


Let's make our first `Dockerfile`

```Dockerfile
# Specify a base image
FROM alpine

# Download and install dependencies
RUN apk add --update redis

# Specify a startup command
CMD ["redis-server"]
```

In the above snippet, `FROM`, `RUN`, `CMD` are the instructions and whatever follows is the argument.

Explanation:
1. We choose our base image as `alpine`. It is basically a linux distribution whose image size is just around 5 MB.
2. Now, we install `redis` into the image.
3. Finally, run the command `redis-server`

Now, to compile the Dockerfile and convert it into an image, we use the command `docker build .`

**_Important Tip_**: Imagine instructions in `Dockerfile` as a series of steps that you would perform on an actual system(without an OS) to run an application.
For instance, consider the code snippet we discussed above. Suppose you need to run a redis server on a system that doesn't even have an OS. You will be required to perform the following steps.
1. Install OS: Alpine
2. Install redis
3. Run redis-server


### Understanding `docker build .`

By using this command, we basically give the Dockerfile to the docker-cli which intern provides it to the docker-server.
The docker-server reads the configurations and convert it into an usable image.

The `dot` in `docker build .` represents the context. Which is basically the set of files and folders which we want to include in this container.

Steps performed:

1. While executing `From <image-name>`, the docker checks the cache if the image is already present in the system or not. If not then it downloads it from Docker Hub.
2. Now for every instruction, the following happens
   1. Consider intial downloaded image
   2. Make a temporary container
   3. Copy the file system of the image into the container
   4. Execute the command
   5. Take a snapshot of the file system after the command gets executed
   6. Shut down the temporary container
   7. The image now has the recent file system snapshot
   8. For the next command, the new temporary image is taken into consideration. The process repeats for all other instructions.
3. For the instruction `CMD <command>`, the process is almost the same. Just instead of executing anything, docker tells the container to run the mentioned command if it ever runs in future.
4. Finally the output is the image generated after the execution of the last instruction.

### Rebuilding using cache

While we build an image, docker keeps adding all the intermediate images into cache. So the next time we build an image, the docker will use the cache instead of executing the commands.

For example consider this Dockerfile

```Dockerfile
# Specify a base image
FROM alpine

# Download and install dependencies
RUN apk add --update redis

# Specify a startup command
CMD ["redis-server"]
```

Upon executing `docker build .`, we get the following output:

```bash
╰─$ docker build .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM alpine
 ---> `a24bb4013296`
Step 2/3 : RUN apk add --update redis
 ---> Running in 0fd5e4b596ea
fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/community/x86_64/APKINDEX.tar.gz
(1/1) Installing redis (5.0.9-r0)
Executing redis-5.0.9-r0.pre-install
Executing redis-5.0.9-r0.post-install
Executing busybox-1.31.1-r16.trigger
OK: 7 MiB in 15 packages
Removing intermediate container 0fd5e4b596ea
 ---> `79396627181b`
Step 3/3 : CMD ["redis-server"]
 ---> Running in 1e5661569a17
Removing intermediate container 1e5661569a17
 ---> e741c19dbebe
Successfully built `e741c19dbebe`
```

Upon running the command again,

```bash
╰─$ docker build .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM alpine
 ---> `a24bb4013296`
Step 2/3 : RUN apk add --update redis
 ---> Using cache
 ---> `79396627181b`
Step 3/3 : CMD ["redis-server"]
 ---> Using cache
 ---> e741c19dbebe
Successfully built `e741c19dbebe`
```

Compare both the outputs and see how docker uses the cache.
After executing the second command, the image id is `79396627181b`. When we run the docker build command again, see that docker uses the same image from the cache instead of building it again.

Now consider the following Dockerfile:

```Dockerfile
# Specify a base image
FROM alpine

# Download and install dependencies
RUN apk add --update redis
RUN apk add --update gcc

# Specify a startup command
CMD ["redis-server"]
```

Here, the docker will use the cache till the second command(installing redis) but since the 3rd command is new, it gets executed.

What will happen if we switch 2nd and 3rd instr in the above Dockerfile? Try it out.

### Tagging docker images

What if I ask you to remember this image id `e741c19dbebe` in order to run a new container everytime. Difficult right?

Here is why we have a concept called tagging. In layman's term we basically tag the image with our custom name which can be used to run a container.

The command will look as follows: `docker build -t <name> .`

Conventionally, the `tagged name` has the following format: `<your-docker-id>/<project-name>:<version/latest>`

It's good to follow the convention however, you may tag the image with any name you want.

`docker  build -t test-image:latest .`

`docker run test-image`

or

`docker build -t learn-docker/redis:latest .`

`docker run learn-docker/redis`


***Note:*** Wherever we don't mention the version(or latest), the latest version of the image is considered.


### Generating image out of a container

Till now we've seen how to build an image and run a container out of it. The following steps help us to generate an image out of a container.

1. Launch a container with shell. `docker run -it alpine sh`
2. Run all the required commands
3. On another terminal, use `docker ps` to get the id of the current running container
4. Run `docker commit -c 'CMD ["redis-server"]' <container-id>`. This commands helps us to mention the startup command of the container. It prints the id of the new image.
5. Run `docker run <new-image-id>` to launch a container with the newly created image.

## Dockerizing Node Application - Project

Let's get all the theory in practice. We shall be making a Node Application and then Dockerizing it.

Head over to [Project-1](/Docker/project-node-app/README.md)
