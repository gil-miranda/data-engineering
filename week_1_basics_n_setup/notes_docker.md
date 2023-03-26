# Docker + PostgreSQL

## Docker

### What is Docker?
`Docker` is an open-source platform that allows you to build, test, deploy and manage applications in a containerized way. In short, it is a containerization platform.<br>

### And what is a Container?
Containers are much like traditional Virtual Machines (VMs), but with some perks. They do not ship a full functioning OS like a VM, this enables containers to be `light weight`. 

### Why use Docker?
With Docker, we get a single object/application that can reliably run in any machine. It's faster to ship and deploy applications.

### Tools and terms

#### DockerFile
`DockerFile` automates the process of Docker image creation. It's a simple text file containing `CLI` instructions for how to build the Docker container.

#### Docker images
`Docker images` contain applications source code and all the tools, libraries and dependencies that the application code needs to run as a container. When we run a docker image, it becomes an instance of the container. It's a read-only object.

#### Docker containers
`Docker containers` are the live, running instances of Docker images. While docker images are read-only files, containers are live, executable.

#### Docker Hub
Public repository of Docker images

### Running Docker
To run a docker container, first one needs to start the `docker daemon`, once the daemon is started it's now possible to run a container from a image:
```bash
docker run {image-name}
```
Example: Running the image `hello-world`
```bash
docker run hello-world
```
This example will output a message, confirming that Docker is sucesfully installed and running.<br>
We can run a working `ubuntu` terminal in a container with the `-it` option, which stands for `interactive`
```bash
docker run -it ubuntu
```
And when we say that a container is `isolated`, and a image is `read-only`, we mean it!<br>
For the sake of a proof, we can start a container with ubuntu, and delete everything with the command
```bash
rm -rf / --no-preserve-root
```
All the files from the `OS` running inside the container will be deleted, but once you close and reopen the container from the same image, everything will be up and running.<br>
Well, but this also means that any configuration we make will be lost once the container is closed. For example, to open a container with the latest version of `Python` installed we can use the following command:
```bash
docker run -it --entrypoint=bash python:latest
```
With the container up and running, we can install the `numpy` package
```bash
python -m pip install numpy
```
and check that it worked
```bash
pip list
```
Once we close this container, and reopen the same image with
```bash
docker run -it --entrypoint=bash python:latest
```
The numpy package is no longer installed on Python.<br>
So, how do we keep a container with everything the application needs? Configuring it with a `DockerFile`!

## Postgres

We can start `POSTGRESQL` service with the following command:
```bash
docker run -it \
    -e POSTGRES_USER="root" \ # Set the variable with the associated value (DB User)
    -e POSTGRES_PASSWORD="root" \ # Set the variable with the associated value (DB Password)
    -e POSTGRES_DB="ny_taxi" \ # Set the variable with the associated value (DB Name)
    -v ny_taxi_postgres_data:/var/lib/postgressql/data \ # Use -v for volume mounting, maps ny_taxi_postgres_data on local machine to /var/lib/postgressql/data on Container
    -p 5432:5432 \ # Use -p for port mapping between local machine and Container
    postgres:13 # Docker build to start
```z