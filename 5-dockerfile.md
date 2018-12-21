#### FROM
尽量使用官方的 image 作为 base image。 
```
FROM scratch # 制作 base image
FROM centos # 使用 base image
FROM ubuntu:14.04
```

#### LABEL

定义 meta 信息
```
LABEL maintainer="jxlwqq@gmail.com" # 维护者
LABEL version="1.0.0"               # 版本
LABEL description="bala bala"       # 描述信息
```

#### RUN

为了代码可读性，复杂的 RUN 可食用 反斜线`\`进行换行。同时为了避免无用分层，尽量将多条命令合并。
```
RUN yum update && yum install -y vim python-dev
```
```
RUN apt-get update && apt-get install -y perl pwgen --no-install-recommends && rm -rf /var/lib/apt/lists/*
```
```
RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
```

#### WORKDIR

设置当前工作目录。有点类似 `cd` 和 `mkdir` 的混合体。
在编写 Dockerfile 时：
* 尽量使用 WORKDIR，不要用 RUN cd。
* 尽量使用绝对路径。

```
WORKDIR /root 
WORKDIR /test # 如果没有则会自动创建
WORKDIR demo  # 相对路径
RUN pwd       # 输出：/test/demo
```
#### COPY and ADD
大部分情况，COPY 优于 ADD。
ADD 除了 COPY 还有额外功能（解压）。
添加远程文件/目录，请使用 curl 或者 wget。
参考：[Dockerfile中的COPY和ADD指令详解与比较](https://blog.csdn.net/taiyangdao/article/details/73222601)
```
ADD hello /
ADD test.tar.gz / # 添加到根目录并解压
WORKDIR /root
ADD hello test/   # /root/test/hello
WORKDIR /root
COPY hello test/  # /root/test/hello
```

#### ENV
设置环境变量
```
ENV MYSQL_VERSION 5.7
RUN apt-get install -y mysql-server= "${MYSQL_VERSION}" && rm -rf /var/lib/apt/lists/*
```

#### VOLUME

存储

#### EXPOSE

网络

#### CMD and ENTRYPOINT

* RUN 执行命令并创建新的镜像层，经常用于安装软件包。

* CMD 设置容器启动后默认执行的命令及其参数。
    * 如果 docker run 指定了其他命令，CMD 指定的默认命令将被忽略。
    * 如果 Dockerfile 中有多个 CMD 指令，只有最后一个 CMD 有效。

* ENTRYPOINT 配置容器启动时运行的命令。

最佳实践：

* 使用 RUN 指令安装应用和软件包，构建镜像。

* 如果 Docker 镜像的用途是运行应用程序或服务，比如运行一个 MySQL，应该优先使用 Exec 格式的 ENTRYPOINT 指令。CMD 可为 ENTRYPOINT 提供额外的默认参数，同时可利用 docker run 命令行替换默认参数。

* 如果想为容器设置默认的启动命令，可使用 CMD 指令。用户可在 docker run 命令行中替换此默认命令。