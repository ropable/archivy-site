---
date: 02-05-21
id: 11
path: Video courses
tags: [Udemy]
title: Docker Mastery
type: note
---

# Useful resources

* Docker Hub: https://hub.docker.com/
* Compose file reference: https://docs.docker.com/compose/compose-file/
* Add your user to the docker group (Linux post-install steps): https://docs.docker.com/install/linux/linux-postinstall/
* Dockerising a Django project: https://medium.com/@theparadoxer02/dockerizing-a-django-project-part-1-a0d3bebd738f
* Building smaller Docker images: https://blog.realkinetic.com/building-minimal-docker-containers-for-python-applications-37d0272c52f3
* USEFUL FOR LEARNING - Play with Docker application: https://labs.play-with-docker.com/
* A curated list of Docker resources and projects: https://github.com/veggiemonk/awesome-docker
* Write Dockerfiles for Python web apps: https://blog.hasura.io/how-to-write-dockerfiles-for-python-web-apps-6d173842ae1d
* Linting Dockerfiles (highlights bugs and suggests improvements): https://www.fromlatest.io/#/
* Dockerfile security best practices: https://cloudberry.engineering/article/dockerfile-security-best-practices/

# Managing Docker Hub creds

* Via pass: https://hackernoon.com/getting-rid-of-docker-plain-text-credentials-88309e07640d
* Useful: https://github.com/docker/docker-credential-helpers/issues/102#issuecomment-388634452
* Setting up pass on git with a gpg key: https://gist.github.com/flbuddymooreiv/a4f24da7e0c3552942ff

To unlock pass keychain:

```bash
pass show docker-credential-helpers/docker-pass-initialized-check
```

# Creating and using Containers

Pull an image from the online registry to local storage:

```
docker pull <IMAGE NAME>
```

Basic run command:

```
docker container run <IMAGE>
docker container run --publish 80:80 nginx
```

* Searches Docker Hub for an image called "nginx" and downloads the latest one (caches it locally).
* Creates a new container based on the image.
* Gives it a virtual IP on a private network in the Docker engine.
* Open port 80 on the host, and forward that to port 80 inside the container.
* Runs the container in the foreground.


```
docker container run --publish 80:80 --detach nginx
```


Same thing, detached. Stop a container:

```
docker container stop <CONTAINER ID>
```


List containers:

```
docker container ls
docker container ls -a
```


Give it a name ("webhost"):

```
docker container run --publish 80:80 --detach --name webhost nginx
```


View logs, use top and stop it:

```bash
docker container logs <NAME>
docker container top <NAME>
docker container inspect <NAME>
docker container stop <NAME>
```


Remove containers:

```
docker container ls -a
docker container rm <ID1> <ID2>
docker container rm -f <ID OF RUNNING CONTAINER>
```


Manage containers assignment (command list):

```
docker container run --publish 8082:80 --detach --name webserver httpd
docker container logs webserver
docker container run --publish 3306:3306 -d -n mysql_db -e MYSQL_RANDOM_ROOT_PASSWORD=true
docker container run --publish 3306:3306 -d --name mysql_db -e MYSQL_RANDOM_ROOT_PASSWORD=true
docker container run --publish 3306:3306 --detach --name mysql_db -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql
docker image ls
docker container ls
docker container rm -f 53b 13f 4c3
docker container ls -a
```


Viewing information about running containers:

```
docker container top <NAME>
docker container inspect <NAME>
docker container stats
```


Getting inside a running container:

```bash
# Start a container interactively and run sh in it:
docker container start -it alpine sh
# Run a bash shell on a running container:
docker container exec -it <NAME> bash
```


Misc. command dump:

```bash
# Display IP address of a running container:
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' <NAME>
# Remove all exited containers:
docker ps -a | grep Exited | cut -d ' ' -f 1 | xargs docker rm
```


Docker container networking: https://docs.docker.com/network/

```
docker network ls
docker network inspect <NAME>
docker network create <NAME>
docker network connect <NETWORK> <CONTAINER>
docker network disconnect <NETWORK> <CONTAINER>
```


CLI app testing assignment:

```
docker image pull centos:7
docker image pull ubuntu:14.04
docker image ls
docker container run --rm -it --name ubuntu ubuntu:14.04 bash
docker container ls -a
docker container run -it --name centos centos:7 bash
docker rm $(docker ps -a -q)
```


DNS round robin test assignment:

```
docker container run --detach --name=search1 --network=my_app_net --network-alias=search elasticsearch:2
docker container run --detach --name=search2 --network=my_app_net --network-alias=search elasticsearch:2
docker container ls -a
docker container run -it --network my_app_net alpine sh
docker container run --rm --network my_app_net alpine nslookup search
docker container run -it --network my_app_net centos bash
docker container run --rm --network my_app_net centos curl -s search:9200
```


# Container images

What's in an image?
*
* App binaries and dependencies
* Metadata about the image data and how to run it
* NOT a complete OS. No kernel, drivers, etc.
* As small as a single executable (compiled binary)
* Official specification: https://github.com/moby/moby/blob/master/image/spec/v1.md

Official Docker registry: https://hub.docker.com

List of "official" Docker images: https://github.com/docker-library/official-images/tree/master/library

Docker images are composed of a series of "layers", which actually represent changes to a file system. When an image is built, it reproduces all of the changes in sequence to recreate the same file system (read-only). Each layer is uniquely identified and only stored once on a host. A container is just a single read/write layers on top of a built image.

Command reference:

```
docker image history <NAME>
docker image inspect <NAME>
docker image build -t <IMAGE_NAME> .
```


Images can be referred to by repository and tag.

An images is defined/built using a Dockerfile. Dockerfiles define the workflow for creating a new image. Reference: https://docs.docker.com/engine/reference/builder/

Tips:

* Place least-frequently-changed stanzas at the top of a Dockerfile, to reduce the amount of images that need to be recreated in a build.
* Most stanzas are inherited via the FROM instruction, but ENV is not.
* Use WORKDIR to change directories in the image.
* Best practices for writing Dockerfiles: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

Dockerfile assignment 1:

```dockerfile
FROM node:6.14.2-alpine
EXPOSE 3000
RUN apk add --update tini && mkdir -p /usr/src/app
WORKDIR /usr/src/app
COPY . .
RUN npm install && npm cache clean --force
CMD ["/sbin/tini", "--", "node", "./bin/www"]
```


Then:

```
docker image build -t custom_node .
```

!Container lifetime & persistent data

* Containers are usually immutable and ephemeral.
* Non-ephemeral data (databases, uploaded files, etc.) needs to be persisted. It is more important than the container itself.
* Data can be persisted using volumes and bind mounts.

Define a named volume when starting a container:

```
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
docker container run -d --name pgdb -e POSTGRES_PASSWORD=pass -p 5433:5432 -v pgdb:/var/lib/postgresql/data postgres:10-alpine
```

Bind mounting maps a host file/dir to a container file/dir. Can't use this in a Dockerfile, must be at container run. E.g.:

```
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v /path/on/host:/path/in/container mysql
```


Assignment command dump:

```
docker pull postgres:9.6.1-alpine
docker pull postgres:9.6.9-alpine
docker container run -d --name pgdb -e POSTGRES_PASSWORD=pass -p 5433:5432 -v psql-data:/var/lib/postgresql/data postgres:9.6.1-alpine
docker container logs -f pgdb
docker container stop pgdb
docker container run -d --name pgdb2 -e POSTGRES_PASSWORD=pass -p 5433:5432 -v psql-data:/var/lib/postgresql/data postgres:9.6.9-alpine
docker container logs -f pgdb2
psql -h localhost -p 5433 -U postgres
docker volume ls
```


Bind mount assignment command dump:

```
docker run -d -p 8082:4000 -v $(pwd):/site bretfisher/jekyll-serve
```

# Container image registries

Docker Hub: https://hub.docker.com/

* The most popular public registry.
* It's Docker Registry, plus some light image building.

Docker Store: https://store.docker.com/

* Download Docker "editions".
* Certified third-party built images.

Docker Cloud: https://cloud.docker.com/

Self-hosted private registry: https://docs.docker.com/registry/

* Running a secured registry: https://training.play-with-docker.com/linux-registry-part2/

!Container resource usage

Quick check:

```
docker stats
```

Limit resource usage (CPU & memory) in Compose definitions: https://docs.docker.com/compose/compose-file/#resources

# Docker Compose

Compose is a tool for defining and running multi-container Docker applications: Compose file format reference: https://docs.docker.com/compose/compose-file/

Compose is configured using YML-formatted files.

```
docker-compose up
docker-compose ps
docker-compose down  # Clean up running containers, networks, etc. Use -v to remove volumes also.
```


Multi-container service assignment command dump:

```yaml
version: "3"

services:
  pgdb:
    image: postgres
    environment:
      - POSTGRES_PASSWORD=pass
    volumes:
      - pgdb-data:/var/lib/postgresql/data
  drupal:
    image: drupal
    ports:
      - "8082:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-themes:/var/www/html/themes
    depends_on:
      - pgdb

volumes:
  pgdb-data:
  drupal-modules:
  drupal-profiles:
  drupal-themes:
```

Using Compose to build images, use the build options: https://docs.docker.com/compose/compose-file/#build

## Multi-container assignment 2

Dockerfile:


```dockerfile
FROM drupal:8.5.4-apache

RUN apt-get update \
    && apt-get install -y git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html/themes

RUN git clone --branch 8.x-3.x --single-branch --depth 1 https://git.drupal.org/project/bootstrap.git \
    && chown -R www-data:www-data bootstrap

WORKDIR /var/www/html
```


Compose file:


```yaml
version: "3"

services:
  pgdb:
    image: postgres:10-alpine
    environment:
      - POSTGRES_PASSWORD=pass
    volumes:
      - pgdb-data:/var/lib/postgresql/data
  drupal:
    build: .
    image: drupal-custom
    ports:
      - "8082:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-sites:/var/www/html/sites
      - drupal-themes:/var/www/html/themes
    depends_on:
      - pgdb

volumes:
  pgdb-data:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
```

# Docker Swarm

Docker Swarm is the built-in orchestration functionality to run "services" as multiple containers each running an identical "task".

Swarm mode uses a distributed consensus to maintain cohesion. Raft consensus overview: http://thesecretlivesofdata.com/raft/

Docs, deploying services to a swarm: https://docs.docker.com/engine/swarm/services/

To enable Swarm mode for a Docker host:


```
docker swarm init
docker info
```


Service commands:

```
docker service create alpine ping 1.1.1.1
docker service ls
docker service update <NAME> --replicas 3
docker service ls
docker service rm <NAME>
```


Swarm commands:

```bash
docker node ls
docker swarm join-token [manager|worker]
# Update a node to a manager:
docker node update --role manager <HOSTNAME>
# Change a node to a worker:
docker node update --role worker <HOSTNAME>
# Deploy a service to the swarm:
docker service create --replicas 3 alpine ping 1.1.1.1
# List services running on a node:
docker node ps <NODE>
# List nodes running a service;
docker service ps <SERVICE>
# Inspect service details:
docker service inspect --pretty <SERVICE>
```

## Scaling a swarm with overlay networking

Creating a swarm using overlay network driver creates a distributed network between all the Docker hosts using that network (kind of a VPN between the hosts).

Ref: https://docs.docker.com/network/overlay/

Swarm services connected to the same overlay network expose all ports to each other. For a port to be accessible outside of the service, it must be published on service create/update. All nodes in a swarm participate in an ingress routing mesh. The mesh enables each node in the swarm to accept connections on published ports to any service running in the swarm. Routing mesh docs: https://docs.docker.com/engine/swarm/ingress/

The routing mesh:

* Routes ingress (incoming) network packets for a service to the proper task.
* Spans all nodes in the swarm.
* Load balances swarm services across their tasks.

Example:

Swarm assignment 1 commands:

```
docker service create --name db --env POSTGRES_PASSWORD=pass --mount type=volume,source=db-data,destination=/var/lib/postgresql/data --network backend postgres:10.4-alpine
docker service create --name worker --network backend --network frontend dockersamples/examplevotingapp_worker
docker service create --name redis --network frontend redis:alpine
docker service create --name vote --network frontend --publish 5000:80 --replicas 2 dockersamples/examplevotingapp_vote:before
docker service create --name result --network backend --publish 5001:80 dockersamples/examplevotingapp_result:before
```

## Swarm Stacks using Compose

Stacks are another abstraction is Docker Swarm. Stacks accept Compose files as their declarative definition for services, volumes and networks. The concept of a Stack is as follows:

Commands:

```
docker stack ls
docker stack services <STACK>
docker stack ps <STACK>
```

## Using secrets in Swarm Services

Docs: https://docs.docker.com/engine/swarm/secrets/

A good example: https://stackoverflow.com/a/42151570/14508

NOTE: when using an external secret mounted at container runtime, environment variables need to be suffixed with _FILE. Example:

```yaml
version: "3.1"
services:
  drupal:
    image: drupal:8.5.4
    build: .
    ports:
      - "8080:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-sites:/var/www/html/sites
      - drupal-themes:/var/www/html/themes
  postgres:
    image: postgres:9.6
    secrets:
      - psql-pw
    environment:
      - POSTGRES_PASSWORD_FILE=/run/secrets/psql-pw
    volumes:
      - drupal-data:/var/lib/postgresql/data
volumes:
  drupal-data:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
secrets:
  psql-pw:
    external: true
```

# Using Compose in the dev/test/prod workflow

* Using multiple Compose files: https://docs.docker.com/compose/extends/#multiple-compose-files
* Using Compose in production: https://docs.docker.com/compose/production/

!Service updates

Reference: https://docs.docker.com/engine/reference/commandline/service_update/

In most cases, containers will be replaced during an update. Service updates will usually need to specify -add or -rm after the configuration being changed. Examples:

```bash
# Update the image used by a service:
docker service update --image myapp:tag <SERVICE>
# Add an environment variable and remove a port:
docker service update --env-add NODE_ENV=production --publish-rm 8080 <SERVICE>
# Change number of replicas for two services:
docker service scale <NAME1>=4 <NAME2>=2
# Deploy changes via a YML file:
docker stack deploy -c <YML FILE> <SERVICE>
# Inspect service details:
docker service inspect --pretty <SERVICE>
```

# Healthchecks in Dockerfiles

Supported in Dockerfile, Compose, docker run and Swarm. Expects exit 0 (OK) or exit 1 (ERROR). There are three container states: starting, healthy, unhealthy.

Docker run healthcheck example (Elasticsearch container):

```
docker run \
  --health-cmd="curl -f localhost:9200/_cluster/health || false"\
  --health-interval=5s \
  --health-retries=3 \
  --health-timeout=2s \
  --health-start-period=15s \
  elasticsearch:2
```

Dockerfile examples (docs https://docs.docker.com/engine/reference/builder/#healthcheck):

```
HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

Postgresql:

```
HEALTHCHECK --interval=5s --timeout=3s \
  CMD pg_isready -U postgres || exit 1
```

Compose example (docs https://docs.docker.com/compose/compose-file/#healthcheck):

```yaml
version: "3.1"
services:
  web:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s  # version 3.4 minimum
```

# Controlling container placement

Docker Swarm has a number of methods of control/constrain the placement of containers on Swarm nodes:

* Labels
* Node service mode
* Placement preferences
* Availability
* Resource requirements

Service constraint examples:

```bash
# Place only on a manager:
docker service create --constraint=node.role==manager nginx
# Place only on non-manager node:
docker service create --constraint=node.role!=worker nginx
# Add a label and constrain to it:
docker node update --label-add=dmz=true <NODE>
docker service create --constraint=node.labels.dmz==true nginx
# Constrain to a particular host:
docker service create --constraint:node==HOSTNAME nginx
# Remove a label:
docker node update --label-rm dmz <NODE>
```

View labels information:

```bash
# View labels and other info on self:
docker node inspect self --pretty
# Inspect another node:
docker node inspect <NODE HOSTNAME OR ID> --pretty
```

Define service constraints in a Compose file:

```yaml
version: "3.1"
services:
  db:
    image: postgres
    deploy:
      placement:
        constraints:
          - node.hostname == dockerswarmhost
          - node.labels.disk == ssd
```

Built-in labels:

* node.id
* node.hostname
* node.ip
* node.role (manager, worker)
* node.platform.os (linux, windows, etc.)
* node.platform.arch (x86_64, arm64, 386, etc.)
* node.labels

Constrain via service modes - default mode is "replicated", but we can use "global" instead. "Replicated" mode means to create the defined number of container replicas on matching nodes. "Global" mode means to create a container on ALL matching nodes.

```
docker service create --mode=global --name test1 --constraint=node.role==worker nginx
```

Placement can also be managed using placement preferences (only current strategy is "spread"):

```bash
# Define a couple of labels on our nodes
docker node update --label-add azone=1 kens-mate-001
docker node update --label-add azone=2 kens-mate-002
docker node update --label-add azone=2 kens-docker-dev
# Create a service and require placement to be spread between labels with different values:
docker service create --placement-pref=spread=node.labels.azone --replicas 2 --name webapp1 nginx
```

Placement preference in Compose files:

```yaml
version: "3.1"
services:
  web:
    image: nginx
    deploy:
      placement:
        preferences:
          spread: node.labels.azone
```

# Node availability

Each Swarm node can have one of three admin-controlled states (only affects if new or existing containers can run on the node):

* active - (default) runs existing tasks, available for new tasks.
* pause - runs existing tasks, not available for new tasks.
* drain - reschedules existing tasks, not available for new tasks.

# Resource constraints

Services can be run with limits set for maximum CPU and memory usage, as well as reservations of minimum guaranteed CPU and memory. Reference: https://docs.docker.com/config/containers/resource_constraints/

```bash
# Reserve CPU and memory:
docker service create --reserve-cpu 1 --reserve-memory 800M mysql
# Limit:
docker service create --limit-cpu 0.25 --limit-memory 150M nginx
# To remove reservations, update them to 0:
docker service update --limit-memory 0 --limit-cpu 0 mysql
```

Compose example (reference: https://docs.docker.com/compose/compose-file/#resource):

```yaml
version: "3.1"
services:
  database:
    image: mysql
    deploy:
      resources:
        limits:
          cpus: '1'
          memory: 1G
        reservations:
          cpus: '0.5'
          memory: 500M
```

# Service logs

Only works for logs that are not shipped to another service (via --log-driver).

Reference: https://docs.docker.com/engine/reference/commandline/service_logs/

```bash
# Return all logs for a service:
docker service logs <SERVICE>
# Return all logs for a task:
docker service logs <TASK ID>
# Return unformatted logs with no truncing:
docker service logs --raw --no-trunc <SERVICE/TASK>
# Return that last 50 logs and follow:
docker service logs --tail 50 --follow <SERVICE/TASK>
# To pipe service logs output to grep, use 2>%1
docker service logs --tail 500 <SERVICE> 2>&1 | grep "Some search text"
# To display the full path of the log for a container:
docker inspect --format='{{.LogPath}}' <CONTAINER ID>
Docker events
docker events
docker events --since 30m
docker events --since 2h10m
docker events --since 2018-06-01
# Filter by event name
docker events --since 1h --filter event=start
docker events --since 1h --filter scope=swarm --filter type=network
```
