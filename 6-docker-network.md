#### Vagrant was unable to mount VirtualBox shared folders

https://github.com/scotch-io/scotch-box/issues/296

```
vagrant plugin install vagrant-vbguest
vagrant vbguest
```

#### network namespace

```bash
docker run -d --name test1 busybox /bin/sh -c "while true; do sleep 3600; done;"
docker run -d --name test2 busybox /bin/sh -c "while true; do sleep 3600; done;"

docker container ls
docker exec `test1-container-id` ip a
docker exec `test2-container-id` ip a

docker exec -it `test1-container-id` /bin/sh
docker exec -it `test2-container-id` /bin/sh
```


```
docker network ls
docker network inspect `network-id`
```

#### link

实际，用的不多！！！仅做了解。

```bash
docker run -d --name test1 busybox /bin/sh -c "while true; do sleep 3600; done;"
# then
docker run -d --name test2 --link test1 busybox /bin/sh -c "while true; do sleep 3600; done;"

# --link 给 ip 起别名，当前与写入一个dns记录
docker exec -it test2 ping test1
```

#### 新建 bridge
```bash
docker network create -d bridge my-bridge
docker network ls
## 指定 network
docker run -d --name test3 --network my-bridge busybox /bin/sh -c "while true; do sleep 3600; done;"

docker network connect my-bridge test2

# 多个容器如果连接到自定义 network bridge 之上的话，可以直接 ping container-name，而无需 --link
```

#### 端口映射
```bash
docker run --name web -d nginx
docker network inspect bridge
# 查看 ip，然后检测
ping 172.17.0.4
telnet 172.17.0.4 80 # 80 端口
curl http://172.17.0.4
```

```
docker stop web
docker rm web

docker run --name web -p 80:80 -d nginx # 端口映射

curl http://172.17.0.4     # ok docker-node1 虚拟机 web 容器对应的ip
curl http://127.0.0.1      # ok docker-node1 虚拟机 本地ip
curl http://192.168.205.10 # ok docker-node1 虚拟机 对应的ip，浏览器 http://192.168.205.10，可以看到 nginx 欢迎页面

```

#### 链接 host 或 none

默认 network name 除了 bridge，还有 host 和 none
```
docker run -d --name test4 --network none busybox /bin/sh -c "while true; do sleep 3600; done;"
docker run -d --name test5 --network host busybox /bin/sh -c "while true; do sleep 3600; done;" # 无独立 namespace，与主机共享，容器与主机可能会有端口冲突的问题。
```

#### 容器间通信

实战 flask-redis：
```bash
docker run -d --name redis redis
cd /home/vagrant/labs/flask-redis
docker image build -t jxlwqq/flask-redis .

docker run -d --link redis --name flask-redis -p 5000:5000 -e REDIS_HOST=redis jxlwqq/flask-redis
# -e 设置环境变量
# -p 端口映射
# -d 后台运行
# --link ip 别名
# --nane 容器名称

curl 127.0.0.1:5000
```

#### 多机通信

概念1：VXLAN 隧道

概念2：overlay 网络

概念3：分布式存储 etcd

[环境构建](./6-docker-network/multi-host-network.md)

```bash
# docker-node1 
docker run -d --name test1 --network demo busybox /bin/sh -c "while true; do sleep 3600; done;"
# docer-node2
docker run -d --name test2 --network demo busybox /bin/sh -c "while true; do sleep 3600; done;"
```
