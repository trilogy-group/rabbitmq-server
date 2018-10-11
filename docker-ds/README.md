# RabbitMQ Docker Development Environment

## RabbitMQ Development Docker image

The DockerImage is created from the Dockerfile based on `ubuntu:18.04` image. Git, .NET Core 2.1, npm and other basic developer tools are available in the image. 

The image is located at `registry2.swarm.devfactory.com/devfactory/rabbitmq/rabbitmq-ds:latest`.  Please refer to the section *Build the DevSpaces Docker image* for more information.

# Quick Start Using Docker-Compose

- Switch to the `{rabbitmq-project-root-folder}/docker-ds` or extract the contents of the RabbitMQ Dockerization artifacts to the  `{rabbitmq-project-root-folder}/docker-ds` directory.
- Open a terminal session to that folder.  Run `./docker-cli start` : Build and run the containers
- Run `./docker-cli exec`
- At this point you must be inside the docker container, in the root folder of the project in the container (i.e.: `/data`). From there, you can build the repository as usual:
    - `make` to build RabbitMQ.  This will generates build contents under  `{rabbitmq-project-root-folder}/deps`
    - See the section *Using RabbitMQ* for details on how to use RabbitMQ Docker container for running the RabbitMQ Server
- When you finish working with the container, type `exit`
- Run `./docker-cli stop` to stop the service or `./docker-cli down` to stop the container and remove the image/network. 


# Quick Start Using DevSpaces

- Switch to the `{rabbitmq-project-root-folder}/docker-ds` or extract the contents of the RabbitMQ Dockerization artifacts to the  `{rabbitmq-project-root-folder}/docker-ds` directory.
- Open a terminal session to that folder.  Run `./devspaces-cli start` : Create DevSpaces collection, deploy the container into DevSpaces environment and sync the `{rabbitmq-project-root-folder}/` root directory with the container `/data` .
- Run `./devspaces-cli exec`
- At this point you must be inside the docker container, in the root folder of the project in the container (i.e.: `/data`). From there, you can build the repository as usual:
    - `make` to build RabbitMQ.  This will generates build contents under  `{rabbitmq-project-root-folder}/deps`
    - See the section *Using RabbitMQ* for details on how to use RabbitMQ Docker container for running the RabbitMQ Server
- When you finish working with the container, type `exit`
- Run `./devspaces-cli stop` to stop the collections or `./devspaces-cli down` to stop the collection and delete the collection from DevSpaces. 


# Work with RabbitMQ Development Docker using Docker-Compose

Change directory to the  `{rabbitmq-project-root-folder}/docker-ds/`.  A helper script `docker-cli` is provided to simplify the commands.  Running `docker-cli` without arguments will give a list of possible options.

## Build and Run the container

In `{rabbitmq-project-root-folder}/docker-ds/` folder, run:

```
./docker-cli start
```
This command will use the Dockerfile to create a Docker image and then start the container in detached mode.

### Build the container

In `{rabbitmq-project-root-folder}/docker-ds/` folder, run:

```
./docker-cli build
```
This command will use the Dockerfile and create a Docker image complete with all the tools to build and run RabbitMQ.

### Run the container

In `{rabbitmq-project-root-folder}/docker-ds/` folder, run:

```
./docker-cli up
```
This command will create a running container in detached mode called `rabbitmq-dev`.
You can check the containers running with `./docker-cli info`

## Get a container session

Run:

```
./docker-cli exec
```

This will launch an interactive `bash` session into the `rabbitmq-dev` container.

## Stop/Remove the container

In `{rabbitmq-project-root-folder}/docker-ds/` folder, run:

```
./docker-cli stop
```
This command will create a stop the container.

```
./docker-cli down
```

This command will stop and remove the container.

## docker-compose.yml

The docker-compose.yml file contains a single service: `rabbitmq-dev`.  We will use this service to build the RabbitMQ sources from our local environment, so we mount the RabbitMQ root project dir to the a `/data` folder:

```
    volumes:
      - ../.:/data:Z
```

## Requirements
The container was tested successfully on:
- Docker 17.05 and up
- docker-compose 1.8 and up


# Work with the DevSpaces for RabbitMQ

Install the DevSpaces CLI from [here](http://devspaces-docs.ey.devfactory.com/installation/index.html#installation) as the first step.

Change directory to the  `{rabbitmq-project-root-folder}/docker-ds/`.  A helper script `devspaces-cli` is provided to simplify the commands.  Running `devspaces-cli` without arguments will give a list of possible options.

The most useful commands are listed below.

## Create, deploy and sync the project root directory to DevSpaces Collection for RabbitMQ

```
./devspaces-cli start
```
This will create the DevSpaces collection for RabbitMQ development, deploy it in DevSpaces development environment then synchronize with the `{rabbitmq-project-root-folder}` with the container `/data` directory.  You can log into the [DevSpaces UI Console](https://devspaces.ey.devfactory.com/home/collections) and checks its collection as well as the DevSpaces Image that it uses and its states.

The `./devspaces-cli start` is the combination of 
###  Create the  DevSpaces collection for RabbitMQ 

```
./devspaces-cli [create|build]
```


###  Run DevSpaces container and Sync the RabbitMQ local repos with the Devspaces Container 

```
./devspaces-cli [bind|up]
```
Deploy the collection in DevSpaces development environment then synchronize with the `{rabbitmq-project-root-folder}` with the container `/data` directory if it is not already done.


## Get a container session for RabbitMQ DevSpaces 

Run:

```
./devspaces-cli exec
```

##  Update the DevSpace collection using the rabbitmq-collection.yaml 

```
./devspaces-cli update
```

##  Stop the DevSpaces collection for RabbitMQ 

```
./devspaces-cli stop
```

##  Stop and Delete the DevSpaces collection for RabbitMQ 

```
./devspaces-cli down
```

##  Obtaining deployment information of the DevSpaces collection for RabbitMQ 

```
./devspaces-cli info
```

## Build the DevSpaces Docker image

The DevSpaces DockerImage has already been pre-created, if you need to recreate it again, in `{rabbitmq-project-root-folder}/docker-ds/` folder, run:

```
./build_ds_img.sh
```
This instruction will create a DockerImage in the Docker Registry `registry2.swarm.devfactory.com` as `registry2.swarm.devfactory.com/devfactory/rabbitmq/rabbitmq-ds:latest`



# Using RabbitMQ Server

To launch the built RabbitMQ Server, simply run 
```
make run-broker
```
This will launch RabbitMQ Broker and bind to the local port `5672`.  The RabbitMQ Broker is available on the container via `5672` and and remotely via the exposed port (You can see the exposed port with the result from `./docker-cli info` or for DevSpaces uses `./devspaces-cli info`.
