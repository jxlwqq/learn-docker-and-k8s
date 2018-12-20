#### 从 Docker Hub 拉取镜像
```bash
# 拉取镜像
docker pull ubuntu:14.04
# 镜像列表
docker image ls
```

#### 本地构建镜像

```bash
mkdir hello-docker
cd hello-docker
vi main.c
```
写入以下代码：
```c
#include<stdio.h>
int main()
{
    printf("hello docker\n");
}
```
编译：
```bash
sudo yum -y install gcc glibc-static
gcc -static main.c -o main
```

```
vi Dockerfile
```
在 Dockerfile 写入：
```
FROM scratch
ADD main /
CMD ["/main"]
```

构建：

```bash
docker build -t jxlwqq/hello-docker .
```
镜像列表：
```bash
docker image ls
```
返回值：
```
REPOSITORY            TAG                 IMAGE ID            CREATED              SIZE
jxlwqq/hello-docker   latest              8f4d70fe988a        About a minute ago   857kB
ubuntu                14.04               f17b6a61de28        4 weeks ago          188MB
hello-world           latest              4ab4c602aa5e        3 months ago         1.84kB
```

```bash
docker history 8f4d70fe988a # IMAGE ID
```
```
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
8f4d70fe988a        2 minutes ago       /bin/sh -c #(nop)  CMD ["/main"]                0B                  
11b5f8ea8ee1        2 minutes ago       /bin/sh -c #(nop) ADD file:90db8d97695a893b7…   857kB                          
```

删除镜像：
```bash
docker image rm -f hello-docker
```
