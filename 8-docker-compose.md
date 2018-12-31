#### 搭建 wordpress
https://hub.docker.com/_/mysql
https://hub.docker.com/_/wordpress
```bash
docker pull mysql
docker pull wordpress

docker run -d --name mysql -v $(pwd)/mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=wordpress mysql:latest

docker run -d -p 8080:80 -e WORDPRESS_DB_HOST=mysql:3306 --link mysql wordpress
```

#### docker compose 多容器批处理 

linux 安装

https://docs.docker.com/compose/install/#install-compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

docker-compose.yml

* networks
* volumes
* services

```yml
version: '3'

services:

  wordpress:
    image: wordpress
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_PASSWORD: root
    networks:
      - my-bridge

  mysql:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - my-bridge

volumes:
  mysql-data:

networks:
  my-bridge:
    driver: bridge
```



```bash
docker-compose up
```

```bash
cd
```

```yml
version: "3"

services:

  redis:
    image: redis

  web:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 8080:5000
    environment:
      REDIS_HOST: redis
```

```bash
docker-compose up -d
```

```bash
version: "3"

services:

  redis:
    image: redis

  web:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      REDIS_HOST: redis

```
--scale 

HAProxy

```bash
cd ./8-docker-compose/scale-lb
docker-compose up -d
docker-compose up -d --scale web=3

```

#### example

```bash
git pull https://github.com/dockersamples/example-voting-app

cd example-voting-app

docker-compose up -d
```

