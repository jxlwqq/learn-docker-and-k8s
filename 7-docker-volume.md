
https://hub.docker.com/_/mysql
````bash
docker run -d --name mysql1 -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql:5.7
docker run -d --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql:5.7
docker volume ls
````

```bash
docker run -d --name mysql3 -v mysql:/var/lib/mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql:5.7
```


挂载
```bash
cd /home/vagrant/labs/docker-nginx
docker build -t jxlwqq/docker-nginx .
docker run -d -p 80:80 --name web jxlwqq/docker-nginx
curl 127.0.0.1
# 或者打开浏览器，访问 http://192.168.205.12
```


```bash
docker rm -f web
docker run -d -p 80:80 -v $(pwd):/usr/share/nginx/html --name web jxlwqq/docker-nginx

# -v 挂载
```

```bash
cd /Users/jxlwqq/go/src/learn-docker/7-docker-volume/labs/flask-skeleton
docker build -t jxlwqq/flask-skeletion .
docker run -d -p 80:5000 --name flask -v $(pwd):/skeleton jxlwqq/flask-skeletion
```