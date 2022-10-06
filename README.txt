From source code
yarn build:ci --api="http://127.0.0.1:9080" -- --directory dist
cp apps/meteor/.docker/Dockerfile apps/meteor/dist
cd apps/meteor/dist
docker build -t oftagrid-server . OR docker buildx build --platform linux/arm64/v8 -t oftagrid-server-pi .

Go to folder with docker-compose.yml
docker compose up -d