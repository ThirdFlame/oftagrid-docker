# Instructions for Building Oftagrid Server on arm64 using Docker Compose

From this folder, the folder containing docker-compose.yml, launch the docker-compose file:
docker compose up -d

Get the container id (container-id) of the mongodb container:
docker container ls

Initiate the mongodb replica:
docker exec [container-id] mongo --eval "rs.initiate()"

The Oftagrid Server should now be running on http://localhost:3000.