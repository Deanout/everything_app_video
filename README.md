# README

# Docker Network
docker network create everything_app

# NGINX+LetsEncrypt
```shell
docker-compose build
docker-compose up -d

docker-compose stop
docker-compose start
```

# Rails
```shell
docker build -t app ./video/.
docker volume create app-storage
docker run -d --rm -it --name video --env-file ./video/.env -v app-storage:/rails/storage --network everything_app app
```

# Another Rails
```shell
docker build -t another_app ./another/.
docker volume create app-storage
docker run -d --rm -it --name another --env-file ./another/.env -v app-storage:/rails/storage --network everything_app another_app
```

# Postgres
```shell
mkdir psql/postgres-data
docker build -t psql ./psql/.
docker run -d --name postgres --env-file ./psql/.env -v postgres-data:/var/lib/postgresql/data --network everything_app psql
