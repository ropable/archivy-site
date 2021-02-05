Personal Archivy database based upon https://github.com/archivy/archivy-docker

Setup:

1. Clone repo
1. Jigger the config and data directories to be writable by uid:guid 1000:1000
1. Set up Docker Swarm stack like so: `docker stack deploy archivy -c achivy.yml`
1. Run the `create-admin` step noted in the repo above to create a user.
1. Open up http://localhost:5111 and log in.
