```bash
docker run ubuntu:14.04
```

容器列表：
```bash
docker container ls
docker container ls -a
```
返回：

```bash
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS                      PORTS               NAMES
ea7f7d7b5012        ubuntu:14.04          "/bin/bash"         7 seconds ago       Exited (0) 5 seconds ago                        gracious_engelbart
```

交互式运行容器：
```bash
docker run -it ubuntu:14.04
```
另开一个命令行窗口执行：
```bash
vagrant up
docker container ls
```
返回：
```
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
258921992af0        ubuntu:14.04        "/bin/bash"         About a minute ago   Up About a minute                       admiring_mclean
```
清除容器：
```bash
docker container rm 258921992af0 # CONTAINER ID 
docker rm 258921992af0 # CONTAINER ID 
# 批量清除
docker rm $(docker container ls -aq)
# 批量清除状态为 Exited 的容器
docker rm $(docker container ls -f "status=exited" -q)
```