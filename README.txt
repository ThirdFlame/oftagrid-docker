Instructions for Building Oftagrid Server using Docker Compose

From the oftagrid-server source code:
yarn build:ci --api="http://127.0.0.1:9080" -- --directory dist
cp apps/meteor/.docker/Dockerfile apps/meteor/dist
cd apps/meteor/dist
docker build -t oftagrid-server . OR docker buildx build --platform linux/arm64/v8 -t oftagrid-server-pi .

Push the docker image to Docker Hub:
docker push thirdflame/oftagrid-server

From this folder, the folder containing docker-compose.yml:
docker compose up -d

Get the container id (container-id) of the mongodb container:
docker container ls

Initiate the mongodb replica:
docker exec [container-id] mongo --eval "rs.initiate()"

The oftagrid-server should now be running on http://localhost:3000.