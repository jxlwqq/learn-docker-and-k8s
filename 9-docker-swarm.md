#### 虚拟机性能

本地配置不高的，建议使用 `docker-machine` 创建；或者使用：

[Play with Docker](https://labs.play-with-docker.com/), A simple, interactive and fun playground to learn Docker

#### RAFT 通俗解释

http://thesecretlivesofdata.com/raft/

#### 3 节点

1 manager 
2 worker

```bash
# swarm-manager node

docker swarm init --advertise-addr=192.168.205.10

# Swarm initialized: current node (lnwbnupg2618s3pn9cjdqbj7u) is now a manager.
# docker swarm join --token SWMTKN-1-4te8t7dpil03zdu1hf8kt8uv8q7vj6r208anizq1t3mk447f85-4ye83isdydtzummh7i4r8yory 192.168.205.10:2377
# To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```
```bash
# swarm-worker1 node and swarm-worker2 node
docker swarm join --token SWMTKN-1-4te8t7dpil03zdu1hf8kt8uv8q7vj6r208anizq1t3mk447f85-4ye83isdydtzummh7i4r8yory 192.168.205.10:2377

# This node joined a swarm as a worker.
```
查看节点
```bash
# swarm-manager node
docker node ls
```

创建 service

docker service create 与 docker run 类似，不过 docker run 适用于单机 docker，在 swarm 模式下（多节点），就不能使用 docker run 了，二用 docker service create代替。

```bash
## manager node
docker service create --name demo busybox sh -c "while true;do sleep 3600;done"
```

```bash
docker service ps demo
```

```bash
docker service ls
```
返回：
```
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
co5hlcx8vmpy        demo                replicated          1/1                 busybox:latest      
```

```bash
docker service scale demo=5
docker service ls
```
返回：
```
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
co5hlcx8vmpy        demo                replicated          5/5                 busybox:latest      
```

5(ready)/5(scale)

```bash
docker service ps demo
```
返回：
```
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
5ktzifuxfmpl        demo.1              busybox:latest      swarm-manager       Running             Running 8 minutes ago                            
zb86jtfusaio        demo.2              busybox:latest      swarm-worker2       Running             Running about a minute ago                       
g5khamwn72hu        demo.3              busybox:latest      swarm-worker2       Running             Running about a minute ago                       
flfj8osilrny        demo.4              busybox:latest      swarm-manager       Running             Running about a minute ago                       
oxxk1u5b1quy        demo.5              busybox:latest      swarm-worker1       Running             Running about a minute ago     
```


```
# worker1 node
docker ps
```
返回：

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
6ce429de5730        busybox:latest      "sh -c 'while true;d…"   3 minutes ago       Up 3 minutes                            demo.5.oxxk1u5b1quy6hln2r19to4ca
```

```bash
# worker2 node
docker ps
```
返回： 
```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
43edbdedeed9        busybox:latest      "sh -c 'while true;d…"   3 minutes ago       Up 3 minutes                            demo.3.g5khamwn72hu8813lall4uc58
7cd5f2c972a2        busybox:latest      "sh -c 'while true;d…"   3 minutes ago       Up 3 minutes                            demo.2.zb86jtfusaiox4u24q87bn536 
```


在 worker2 中删除一个容器：

```bash
# worker2 node
docker rm -f 43edbdedeed9
```

```bash
# manager node
docker service ps demo
```
```bash
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                    ERROR                         PORTS
5ktzifuxfmpl        demo.1              busybox:latest      swarm-manager       Running             Running 42 minutes ago                                         
zb86jtfusaio        demo.2              busybox:latest      swarm-worker2       Running             Running 35 minutes ago                                         
xnjvk4019nsn        demo.3              busybox:latest      swarm-worker2       Running             Running less than a second ago                                 
g5khamwn72hu         \_ demo.3          busybox:latest      swarm-worker2       Shutdown            Failed 5 seconds ago             "task: non-zero exit (137)"   
flfj8osilrny        demo.4              busybox:latest      swarm-manager       Running             Running 35 minutes ago                                         
oxxk1u5b1quy        demo.5              busybox:latest      swarm-worker1       Running             Running 35 minutes ago     
```


#### 删除 service
```bash
# manager node
docker service rm demo
```


## wordpress-mysql 

创建 overlay 通信网络

```bash
docker network create -d overlay demo
```
创建 mysql service
```bash
docker service create --name mysql --env MYSQL_ROOT_PASSWORD=root --env MYSQL_DATABASE=wordpress --mount type=volume,source=mysql-data,destination=/var/bin/mysql mysql
```

创建 wordpress service
```bash
docker service create --name wordpress -p 80:80 --env WORDPRESS_DB_PASSWORD=root --env WORDPRESS_DB_HOST=mysql --network demo wordpress
```

