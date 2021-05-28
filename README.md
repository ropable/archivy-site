# Overview

Personal Archivy database based upon https://github.com/archivy/archivy-docker

## Setup

This assumes that Docker Swarm in installed and initialised:

1. Clone repo locally
1. Jigger the config, data and image directories to be writable by uid:guid 1000:1000
1. Put your own user into the group with guid 1000 too (optional but useful).
1. Set up Docker Swarm stack like so: `docker stack deploy archivy -c docker-compose.yml`
1. Run the `create-admin` step noted in the repo above to create a user.
1. Open up http://localhost:5111 and log in as that user (see note below about port no).

## Notes

* Adjust the port number in config/config.yml and the compose file if required.
* The data in the `elasticsearch_data` Docker-managed volume is disposable.
* Archivy docs: https://archivy.github.io/
