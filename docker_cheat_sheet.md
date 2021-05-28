# Dockee cheat sheet

## Starting and stopping the docker daemon on arch

To start the docker daemon:
```
$ sudo systemctl start docker
```
````

To stop the docker daemon:
```
$ sudo systemctl stop docker
````


## Build an image from a local Dockerfile

To build an image from a Dockerfile, use the following command:
```
$ docker build -t <tag> -f <docker-file> <context>
```

Here `<tag>` is a name to refer to the image for, e.g., running it.
`<docker-file>` is the Dockerfile that defines the image, and `<context>` is
the directory relative to which the Dockerfile runs, e.g., the current directory
`.`.

Note that a tag for an image to be pushed to Docker Hub should contain your
Docker Hub ID, so `<uid>/<tag>` for example `gjbex/fortran-mooc:1.0`.


## Docker Hub authentication

Obtain an access token from Docker Hub and login using:

```
$ docker login --username <uid>
```

The token should be entered at the prompt.

This only has to be done once, and creates a `.docker` directory in your
home directory with a configuration files that holds the otken.


## Push an image to Docker Hub

Once an image is built, it can be pushed to Docker Hub.

```
$ docker push <uid>/<tag>
```


## Pull an image

An image can be pulled from DockerHub using:
```
$ docker pull <tag>
```

Here the `<tag>` is any image on DockerHub, e.g., `continuumio/miniconda3`.


## List images

You can list the images docker has available locally using
```
$ docker images
```


## Running a container

When you have an image, you can run it as a container uusing
```
$ sudo docker run --rm -i -t <tag> <cmd>
```

The `<tag>` is the tag of the image to run, e.g., `continuumio/miniconda3` and
`<cmd>` is the command to run, e.g., `/bin/bash`.

The `--rm` option ensures that the container is removed when it terminates.
The `-i` options attaches a TTY to the container.

To bind a directory, the `-v <host-dir>:<container-dir>` options can be
specified.  Similarly a port can be bound using the
`-p <host-port>:<container-port>`.


## Listing running containers

The containers that are currently running you can use
```bash
$ sudo docker ps
```

To list all containers, run
```bash
$ sudo docker ps -a
```

## Remove a container

To remove a container, use
```bash
$ sudo docker rm <container-id>
```


## Remove an image

Images that are no longer required can be removed using
```
$ sudo docker image rm <tag>
```

## Using docker-compose

A container can also be build and started using docker-compose.  This requires a
`docker-compose.yml` file such as:
```
version: "3.8"
services:
    app:
      build: .
      stdin_open: true
      tty: true
      volumes:
        - ./data:/data
```

The `stdin_open` and `tty` options ensure that the container remains open.

Start the container using:
```bash
$ docker-compose up
```

This will use the `docker-compose.yml` in the current working directory.

To use the running container, first determine its name:
```
```bash
$ docker ps
$ docker exect -ti <name> /bin/bash
```
