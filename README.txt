From source code
yarn build:ci --api="http://127.0.0.1:9080" -- --directory dist
cp apps/meteor/.docker/Dockerfile apps/meteor/dist
cd apps/meteor/dist
docker build -t oftagrid-server .

Go to folder with docker-compose.yml
docker compose up -d