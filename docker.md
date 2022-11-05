# DOCKER

## HELPFUL COMMANDS
```sh
## BASIC RUNNING

# check that docker is working
docker run hello-world

# use a given image to create a container. if not found locally, pull it from the registry
docker run foo

# run nginx image on and connect host port 8085 to container port 80 -> http://localhost:8085
docker run -d -p 8085:80 nginx

# write data stored to container `/var/lib/mysql` to `/your/dir` on your host machine
docker run -v /your/dir:/var/lib/mysql -d mysql:5.7

# allow container to be stopped with ctrl-c if not using -d switch (-it) and remove once stopped (-rm):
docker run --rm -it -p 8082:80 webserver

# provide an environment variable (-e); if provided in dockerfile, it will be overwritten with this value
docker run -d -e host=www.bing.com --rm pinger

# configure a long-running service to restart unless manually stopped with `docker stop` etc
docker run -d -p 8080:80 --restart unless-stopped nginx

# stop a container
docker stop [containerid/name]


## WHAT'S GOING ON

# list containers that are running
docker ps

# list containers that are running and that are stopped
docker ps -a

# view running container info with resource consumption
docker stats

# view logs for a container, including those that have stopped
docker logs [containerid/name]

# view logs for the past 10 seconds. you can also use --from, --until, and --tail switches
docker logs --since 10s [id]

# get detailed info about a container
docker inspect [containerid/name]


## CLEAN YOUR MESS

# remove stopped containers, without asking for confirmation
docker container prune (-f)

# compare prune to this, which will forcefully remove *running* containers using sigkill. prune only removes stopped containers
docker rm -f [id]

# remove ALL containers. follow with `docker system prune` (why? what does system prune do?)
docker rm -f $(docker ps -qa)

# remove unused volumes (not referenced by any containers)
docker volume prune -f

# remove dangling images only
docker image prune -f

# remove all unused images (not referenced by any containers)
docker image prune -a/--all

# remove all unused containers, networks, images (both dangling and unreferenced using -a), and optionally, volumes
docker system prune -fa --volumes

# burn it with fire
docker kill gitlab-runner
docker rm gitlab-runner


## IMAGE MANAGEMENT

# get the latest of an image you might already have
docker pull foo

# view all local images
docker image ls

# remove image
docker rmi [imageid]
docker rmi webserver:latest

# build an image with name (-t) of hello using . as build path
docker build -t hello .

# build an image with name of hello using . as build path and specified dockerfile location
docker build -t hello . -f /path/to/dockerfile


## PUBLISH AN IMAGE

# build of course
docker build [...]

# tag the image if the build didn't give it the appropriate tag
docker tag hello my-repo/hello

# login
docker login

# and push!
docker push hello



```

## OVERVIEW
* check out [this course](https://www.educative.io/courses/docker-for-developers)

### WHAT DOCKER?
* docker is a platform to build, deploy, run, update, and test applications
* it is standardized and repeatable -- a docker app runs the same on any machine regardless of OS/env
* "it works on my machine" -> "then we'll ship your machine"
* docker can be thought of as a tool to create a disposable computer -- you can create the computer over and over again, across many instances, or with small adjustments
* **docker vs kubernetes**
	- docker is a continuous runtime whereas kubernetes is a platform for running and managing containers from many container runtimes -- it's a "meta-docker" of sorts
	- kubernetes supports numberous runtimes including docker, containerd, CRI-O, and kubernetes CRI (container runtime interface)
	- one metaphor is that kubernetes is an "OS" and docker is an "app"


### WHY DOCKER?
* deployment
	- containers make deployment easy: just run a new container, direct users to it and trash the old one
	- better, faster, more automated CI/CD
	- simplified hosting environment from devOps perspective: runtime, tech, library, and OS needs are all simplified -- different apps have their own dependencies in their own container with no need to manage them on a server-wide scale and risk breaking other apps.
	- ex: you want to host a Node.JS app on the same server as some PHP apps, but the new app uses Node.JS runtime together with a package that, when installed, changes a dependency used by the PHP runtime. docker containers to the rescue!
* scaling:
	- typically to support high usage, you'd duplicate the server, the app and all it's dependencies across many servers, then put a **reverse proxy** in front of the service to split the load between them. this makes deployment even harder
	- containers are based on images, and we can have many containers spun up from the same image with the same dependencies etc
	- when using an **orchestrator** you need merely state how many containers you want and the image name, and the orchestrator creates that many containers on your servers
	- the reverse proxy model then works the same to distribute traffic across all docker servers/containers
	- the orchestrator/registry/reverse proxy model also helps make for seamless upgrades/deployments as containers are cycled through
* allows you to serve an app without having to install anything directly on your computer (except docker, of course)
* overall, it provides a **standardized** approach to code development and deployment including:
	- common build chain
	- common image storage
	- common deploy and scale up methodology
	- common container hosting
	- common container control and monitoring
	- common container versioning methodology

### KEY CONCEPTS
* **containers**: what we want to run and use to host our apps in docker
	- containers can be though of as isolated machines, or VMs
	- a container runs inside the docker host, isolated from other containers and the host OS
	- a container contains everything needed to run: OS, packages, runtimes, files, env variables, stdin, stout
	- a typical docker server would host many containers
	- containers should be stateless -- they should not store any persistent data (because they're not persistent), and if you're running multiple load-balanced containers, their data needs to be uniform
* **images**: a container template which describes everything needed to create a container
	- you may create many containers from a single image
	- an image consists of sub-parts that may be reused when a new version is built if they are the same
* **registries**: where images are stored
	- a registry may contain multiple images, which themselves are used to create multiple containers
* **volumes**: where persistent data is kept -- containers are disposible and temporary, so using a volume allows the data to persist


### COMMANDS
* **run**: asks docker to create and run a container based on a given image. if the image isn't already present, it will be downloaded from a registry. the image may specify some outputs be generated to the console. then the container is stopped
* **pull**: images are pulled from the registry if they're not present locally, but the `pull` command forces an image to be downloaded, whether it's already present or not
	- use pull to get `latest` -- `docker run` does *not* pull latest if it has an image already present locally


### LIFECYCLE
* generally you'd do something like
```sh
# clone, pull and run
docker run --name repo alpine/git clone \
	https://github.com/docker/getting-started.git
docker cp repo:/git/getting-started/ .
```
or
```sh
# build
cd getting-started
docker build -t docker101tutorial .

# run
docker run -d -p 80:80 \
	--name docker-tutiorial docker101tutorial

#share
docker tag docker101tutorial /docker101tutorial
docker push /docker101tutorial
```


### GENERAL DOCKER STUFF
* did you know that docker is an api service? so when you use the command line (or the GUI), it's just sending calls to the api -- you could just do it all from postman if that made sense for some reason
* there are different flavors/sizes of docker depending on purpose/needs:
	- dev machine: Docker Engine Community or Docker Desktop
	- small server: Docker Engine Community
	- serious stuff: Docker Engine Enterprise or Kubernetes


## LONG-LIVED CONTAINERS
* we might run a docker container to perform some actions and then it's done, or we might use them as servers
* there are two differences between short-lived and long-lived containers:
	- long vs short lived (duh)
	- long lived listen for incoming network connections
* a docker container can be left running with the **detach** switch: `docker run -d alpine ping www.docker.com`
* *HOWEVER* long-lived containers should still be treated as disposible stateless things


### LISTENING FOR CONNECTIONS
* a container runs in isolation and must be configured to listen for incoming connections
* a port must explicitly be opened on the host machine and mapped to a port on the container
* for example, we have an NGINX web server that listens to port 80 by default. we use the -p switch with the `run` command to specify host/container ports:
	`docker run -d -p 8085:80 nginx` -> hosts page on http://localhost:8085


## DATA
* docker containers are just little temporary, throwaway things, so what if you need persistent data?
* a container dies when it finishes, when it's manually killed, when the host machine restarts, when the container is moved from one node to another, etc -- and all the app's data is lost
* also if you have multiple container instances up in a load-balancing scenario, the data will be different for each, resulting in an inconsistent user experience
* therefore, containers should be stateless
* data should be stored in an external relational database, or a distributed cache (like redis). but you can also store files in a third place where they are persisted: volumes
* a **volume** is a map from inside the container to persistent storage. persistent storage can be:
	- on the host machine
	- azure file storage on azure or amazon s3 on aws
* example: run a mysql db and write any data stored to `/var/lib/mysql` in the container to `/your/dir` on your host machine. the data will still be in `/your/dir` when the container dies
	`docker run -v /your/dir:/var/lib/mysql -d mysql:5.7`


## IMAGES
* images contain the instructions needed to create a container, including the base image, files and image layers
* image layers are subcomponents of an image which may be reused when a new version is built if they are the same
* a docker image is created using the `docker build` command along with a dockerfile


### DOCKERFILES
* a **dockerfile** contains information on how the image should be built
	- dockerfiles can have any name but it makes it easier for other folks to understand what it is to just call it 'dockerfile'
	- dockerfiles begin with `FROM` because every image is based on another image, so `FROM debian:11` will kick off your image with debian linux
	- dockerfiles are therefore extensible and this makes them very powerful -- it's easy to build on a complex image
	- a very basic dockerfile that prints hello, world from a linux cmd is:
		```dockerfile
		FROM debian:11
		CMD ["echo", "Hello, world!"]
		```

#### INSTRUCTIONS
* **FROM**: the first instruction - determines the base image and creates the first layer
* **WORKDIR**: define the working directory in which the subsequent instructions will be run within the container. multiple workdir instructions can be made and must be relative to the previous workdir instruction
* **ENV**: set a default env var. this can be overridden with the `-e` switch
* **COPY**: copies a file from the build context to a destination within the image
* **RUN**: executes a given command on top of the current image and creates a new layer by committing the result
* **CMD**: set default command. there can only be one CMD per dockerfile, if there are more, only the last one executes. can be overridden when the container is run
* **EXPOSE**: *documentation only*. documents which port the app will listen to within the container. use the `-p` switch on build to actually wire up ports
* **VOLUME**:
* **ENTRYPOINT**: configure the container to run as an executable

### BUILDING IMAGES
* build an image with name (-t) of hello using . as build path. omitting a name will generate a unique id instead
	`docker build -t hello .`
* the image can then be run like any other image on the host:
	`docker run --rm hello`

### IMAGE FILES
* images contain other files, say like an index.html page
* you can use a statement in the dockerfile to copy a file from the build context to a destination within the image
	```dockerfile
	FROM nginx:1.15
	COPY index.html /usr/share/nginx/html
	```
	- other files in the build environment will *not* be available to the image or container unless you've used the COPY instruction
* then we can build and run our image:
	```sh
	docker build -t webserver .
	docker run --rm -it -p 8082:80 webserver
	```

### TAGGING IMAGES
* images can be tagged with a publishing name like: `<repository_name>/<name>:<tag>`
* tags are optional and when omitted, it is `latest` by default. the same goes for `run` and `pull` commands -- if tag is omitted, `latest` is assumed
* `repository_name` can be a dns entry or the name of a registry in the docker hub
* if images are only local (not published to a registry) their names are simply `<name>:<tag>`
* using `latest` is fine for a very simple CI/CD scenario where you:
	- update source
	- build a new image with latest tag
	- run a new container with latest
	- kill previous container
* however you certainly might have more nuanced needs, like:
	- be able to roll back to a previous image if there's an issue
	- run different versions in different environments (dev, test, prod, etc)
	- run different versions at the same time, routing some users to latest and some to prev (**canary release**)
	- deploy different versions to different users and be able to run whatever version on your dev machine to support them
* common tagging methods include
	- version number e.g. hello:1.0, hello:1.1, hello:2.0
	- git commit tag e.g. hello:2cd7e376, hello:b43a14bb
* tags are provided on build as part of the name argument:
	`docker build -t hello:1.1 .`
* tags are also used as part of the FROM statement in your dockerfile -- `FROM nginx:1.15`
	- it is NOT recommended to ever use latest in your dockerfile. docker is all about reproducible images, and using `latest` makes it not. also, `latest` is only evaluated during the build command, so that's the only time `latest` matters. this makes for a potentially confusing situation
* tags can be added after build with `docker tag` - this image will now be known by the build machine as both `hello` and `my-repo/hello`
	`docker tag hello my-repo/hello`

### ENVIRONMENT VARIABLES
* just like anything else, a container's inputs and outputs are likely to vary depending on what server/environment it's deployed to
* the tech you use inside your container will determine how you access environment variables. for instance, to get a `name` env variable, you might do:
	```
	Linux shell				$name
	.NET Core				.AddEnvironmentVariables();
	Java					System.getenv(“name”)
	Node.JS					process.env.name
	PHP	.$_ENV[“name”]		Python	os.environ.get(‘name’)
	```
* container env variables can be set from several sources
* to provide an environment variable value at runtime, you can use `-e name=value` in the `docker run` command
* you may also want to define a default value for an env variable -- this can be done in the dockerfile using the `ENV` instruction. the default value can then be overridden in the `docker run` command
* however, even though you can provide vars in the `docker run` command, it's much easier to add an ENV instruction for each env variable your image expects - and it helps document your image

### VOLUME CONFIGURATION
* the `VOLUME` instruction can be used to specify a volume location
* the specified path is internal to the image
* when the `docker run` command is run, the `-v` switch can be used to map this directory to a volume on the host system. if the volume is not mapped to an external store, the data will be stored inside the container

### NETWORKING
* the `EXPOSE` instruction indicates what port(s) should be opened for the services to listen
* using this instruction is purely for documentation purposes -- it does *NOT* open a port to the outside world
* to actually make use of the port, anyone who creates a container needs to explicitly bind that port to an actual port of the host machine using the `-p` switch



## REGISTRIES
* there are lots of images available on the [Docker Hub](https://hub.docker.com/) -- this is what is used by default
* **private registries** can also be configured -- options for privage registry hosting include:
	- docker hub
	- azure container registry
	- gitlab
	- host your own using the registry image on a docker-enabled machine
* registries make your built image available to other machines/users/servers
* a registry consists of
	- images
	- tags for those images
	- an http api that allows the pushing/pulling of images
	- TLS-secured connection to prevent MITM attacks

### REPOSITORIES
* a repository contains the versioned/tagged images for one app/image

### PUBLISHING
* when an image is published to a registry, its name must be:
	`<repository_name>/<name>:<tag>`
* tag is optional; when missing, it is considered to be latest by default
* repository_name can be a registry DNS or the name of a registry in the [Docker Hub](https://hub.docker.com/)
* publishing is a three-step process:
	- build with the appropriate name:tag (`docker build`)
	- log in to the registry (`docker login`)
	- push the image into the registry (`docker push`)
* public images are hosted for free on the Docker Hub -- paid plans available for private images

### IMAGE OPTIMIZATION
* you'll want to make sure your image is as small as possible to reduce push/pull times and minimize the space that images take up on the build/registry/serving machine
* things that influence the size include:
	- the files in your image
	- the base image size
	- image layers
* you can check your image size in the `docker image ls` output
* to minimize size:
	- only copy the files you need, e.g. do not do this: `COPY . .`. use separate COPY instructions
	- `.dockerignore` files can be used to exclude files from the image build, e.g.
		```dockerfile
		# Ignore .git folder
		.git
		# Ignore Typescript files in any folder or subfolder
		**/*.ts
		```
	- when using package managers like NPM, NuGet, apt, and so on, make sure you run them while building the image. It will avoid sending them as the context of the image, and it will allow the layer caching system to cache the result of running them. As long as the definition file doesn’t change, Docker will reuse its cache.
	- use the smallest base image possible. for example, there is a smaller version of debian called `debian:8-slim`. `nginx:1.15-alpine` is also much lighter than `nginx:1.15`
	- partial images are cached at each build step -- this is very effective as these parts are reused at different times to improve efficiency
		* when building, common cached parts are used during build
		* when pushing a new version, the common image parts are not pushed
		* when pulling from a registry, the common part you already have is not pulled
	- docker skips steps up until the first instruction that actually changes something -- you can cheat the system by intentionally ordering the dockerfile and putting the instructions most likely to change at the end of the file

## DEPLOYING WITH DOCKER
* here is a sample dockerfile for an asp.net core application which restores nugets and builds the dlls with necessary dependencies. it does the following
```dockerfile
FROM microsoft/dotnet:2.2-sdk AS builder
WORKDIR /app

COPY . .
RUN dotnet restore
RUN dotnet publish --output /out/ --configuration Release

EXPOSE 80
ENTRYPOINT ["dotnet", "aspnet-core.dll"]
```

### MULTI-STAGE DOCKERFILES
* you can have multiple `FROM` instructions wherein the instructions following the first isolate just the parts of the prior build that we need
* for example, we might need a large sdk to build our app, but in the end we only need the executable. we can make sure our image only contains the necessary layers with something like:
```dockerfile
# use a chonky sdk create the application build
FROM microsoft/dotnet:2.2-sdk AS builder
WORKDIR /app

COPY *.csproj  .
RUN dotnet restore

COPY . .
RUN dotnet publish --output /out/ --configuration Release

# only include the build output in our image, and use a lighter base image
FROM microsoft/dotnet:2.2-aspnetcore-runtime-alpine
WORKDIR /app
COPY --from=builder /out .
EXPOSE 80
ENTRYPOINT ["dotnet", "aspnet-core.dll"]
...
```
* the image produced by second dockerfile weighs only 9% of the first image!

### PLATFORM BOILERPLATES
* find platform-specific boiler plates [here](https://bitbucket.org/epobb/dockerbookfiles/src/master/common-development-profiles/demos/)


### RUNNING CONTAINERS
* when running a container, you can set a restart mode, which tells docker what to do when a container stops
* restart mode is set with switch `--restart`
* it is tempting set restart mode to `always` -- when the container stops, it will restart to maintain uptime -- but this creates issues if you're trying to manually stop the container.
* to restart the container always *unless* you manually stop it, use `unless-stopped`:
	`docker run -d -p 80 --restart unless-stopped nginx`

### MONITORING AND RESOURCES
* `docker stats` provides simple monitoring including running containers and resource consumption stats -- similar to `docker ps` but with resource info
* docker consumes disk space in various ways:
	- stopped containers that were not removed with `--rm` switch
	- unused images
	- dangling images: images with no name. this happens when you `docker build` with the same tag as previous; the old image becomes a dangler
	- unused volumes
* some ways to reclaim disk space:
	```sh
	# remove stopped containers only
	docker container prune -f
	# remove unused volumes (not referenced by any containers)
	docker volume prune -f
	# remove dangling images only
	docker image prune -f
	# remove all unused images (not referenced by any containers)
	docker image prune -a/--all
	# or do it all, including unused images:
	docker system prune -fa --volumes
	```

### ORCHESTRATION
* managing and monitoring containers can be tedious
* with docker, **rolling updates** are preferred to deploy new app instances, route users to them, then stop old ones
* orchestration tools can be used to manage rolling updates by setting up reverse proxies and updating the container routing
* orchestration tools include **Docker Swarm** and **Kubernetes**
* with these tools, you create a cluster of servers (or just one) then feed your orchestrator with a file that states which containers you want, how to expose them to traffic, and how many containers should be run for each image
* a sample Kubernetes file which creates a load-balanced core server with two container instances, exposed and handling env variables
	```
	apiVersion: apps/v1beta1
	kind: Deployment
	spec:
	replicas: 2
	template:
		metadata:
		labels:
			app: core-server
		spec:
		containers:
		- image: learnbook/aspnetcore-server:1.1
			ports:
			- containerPort: 80
			env:
			- name: ASPNETCORE_ENVIRONMENT
			value: "Release"
	---
	apiVersion: v1
	kind: Service
	spec:
	type: LoadBalancer
	ports:
	- port: 80
	selector:
		app: core-server
	```


