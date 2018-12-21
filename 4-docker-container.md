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

构建镜像的两种方式：

```bash
docker container commit # 不推荐
docker image build # 推荐通过 Dockerfile 来构建镜像
```

方法一：
```bash
docker pull centos
docker run -it centos
yum install -y vim # 安装 Vim
exit
```

```bash
docker container ls -a
```
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
21d2b62a2b54        centos              "/bin/bash"         7 minutes ago       Exited (0) 33 seconds ago                       zealous_swartz
```
```bash
docker container commit zealous_swartz jxlwqq/centos-with-vim
docker image ls
```
```bash
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
jxlwqq/centos-with-vim   latest              a31309fac0a5        16 seconds ago      327MB
jxlwqq/hello-docker      latest              8f4d70fe988a        22 hours ago        857kB
centos                   latest              1e1148e4cc2c        2 weeks ago         202MB
ubuntu                   14.04               f17b6a61de28        4 weeks ago         188MB
hello-world              latest              4ab4c602aa5e        3 months ago        1.84kB
```
```bash
docker history a31309fac0a5
docker image rm a31309fac0a5
```

方法二：
```bash
mkdir centos-with-vim
cd centos-with-vim
vi Dockerfile
```
写入：
```
FROM centos:latest
RUN yum install -y vim
```
构建：
```bash
docker build -t jxlwqq/centos-with-vim .
```
