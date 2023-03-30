# Docker Getting Started Application

The content of this repository is based on the
https://docs.docker.com/get-started/
article.

## Commands

```bash
docker build -t getting-started .
docker run -dp 3000:3000 getting-started
docker tag getting-started carltonj2000/getting-started
docker push carltonj2000/getting-started
docker run -dp 3000:3000 carltonj2000/getting-started

docker volume create todo-db
docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
docker volume inspect todo-db
# enable docker desktop file sharing before running the below
docker run -dp 3000:3000 \
    -w /app -v "$(pwd):/app" \
    node:18-alpine \
    sh -c "yarn install && yarn run dev"
docker logs -f <container-id>
```

### Multi-Container

```bash
docker network create todo-app
docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:8.0
docker exec -it <mysql-container-id> mysql -p
mysql> SHOW DATABASES;
docker run -it --network todo-app nicolaka/netshoot
> dig mysql
> exit
docker run -dp 3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"
# open browser, add todos, and verify with command below
docker exec -it <mysql-container-id> mysql -p todos
mysql> select * from todo_items;
```

### Compose

```bash
docker compose up -d
docker compose logs -f app
docker compose down
```

### Layering / Caching / Multi-Stage Builds

```bash
docker image history getting-started

```

## Tests

```bash
docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"

```
