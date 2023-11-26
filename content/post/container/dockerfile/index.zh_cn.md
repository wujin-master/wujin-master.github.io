---
title: 'Dockerfile详解'
date: 2023-11-19T17:35:51+08:00
lastmod: 2023-11-19T17:35:51+08:00
draft: false
---
# Dockerfile详解

Dockerfile用来制作容器镜像。

内容包括注释和指令

注释以 # 开始，后跟注释信息

指令以关键字开始，后跟指令参数

本文结合自身的经验，将指令分为常用指令和不常用指令，以供参考

## 一、常用指令

### FROM
FROM指令用来声明一个已存在的镜像，作为Dockerfile的基础镜像
```
FROM <IMAGE>
FROM <IMAGE>:<TAG>
```
~~FROM指令必须是Dockerfile的第一个指令~~ 这是我早期的错误认识，实际上FROM不必为第一个指令, ARG指令可以在FORM之前。

### ARG
ARG指令用来声明构建参数，
```
# 仅声明
ARG KEY

# 声明并赋默认值
ARG KEY=value
```
使用参数时`${KEY}`

当构建命令`docker build`中使用`--build-arg KEY=newValue`时，会以构建命令中的值为准。

正如FROM中所写，ARG可以在FORM之前，但在FORM之前由ARG声明的参数只能在FROM中使用。实践过程中，为了解决 “如何通过在给dockerfile中传递不同的基础镜像，以实现一个dockerfile可以制作出x86架构的容器镜像和arm架构的容器镜像” 这个问题时，发现了ARG指令这个特性。

### MAINTAINER
MAINTAINER指令用来描述镜像的作者信息，应包括镜像的所有者和联系方式
```
MAINTAINER <NAME>
```
### RUN
RUN指令用于指定构建镜像时运行的命令，有两种模式

```
# shell模式
RUN <COMMAND>

# exec模式
RUN [ "executable", "param1", "param2" ]
```
shell模式默认使用/bin/sh -c COMMAND来执行COMMAND命令

exec模式可以指定其他的shell命令运行，如：

使用bash模式执行命令
```
RUN [“/bin/bash”, “-c”, “echo hello”]
```
如果直接使用其他指令执行命令，则默认为shell模式，如：
```
RUN chmod +x file
```
### EXPOSE
EXPOSE声明容器使用的端口，支持多个
```
EXPOSE <PORT>
```
使用docker启动容器时，不会自动打开端口，所以还需 -p hostPort:containerPort 参数来指定主机端口端口到容器端口的映射，也可使用 -P 参数将主机端口随机映射到容器的80端口

### COPY
COPY指令将目录或文件复制到镜像中。

源文件路径使用Dockerfile相对路径，目标路径使用绝对路径。
```
COPY <src> <dest>

# 当文件路径包含空格时，需使用以下格式
COPY ["<src>" "<dest>"] 
```
### ADD
ADD指令将目录或文件复制到镜像中。

源文件路径使用Dockerfile相对路径，目标路径使用绝对路径。
```
ADD <src> <dest>

# 当文件路径包含空格时，需使用以下格式
ADD ["<src>" "<dest>"] 
```
与COPY指令不同的是，ADD指令含有解压功能，如果只是为了复制文件，建议使用COPY。
### WORKDIR
WORKDIR指令用来设置容器内部的工作目录，当目录存在时等价于`cd dirPath`命令，当目录不存在时等价于`mkdir && cd dirPath`。
### ENV
ENV指令用于设置环境变量，有两种形式
```
ENV <KEY> <VALUE>
ENV <KEY>=<VALUE>
```
### CMD
CMD指令用于提供容器运行的默认命令，如果在docker run时指定了运行的命令，则CMD命令不会执行，有三种模式
```
# shell模式
CMD <command> ()

# exec模式
CMD [ "executable", "param1", "param2" ] ()

# 与ENTRYPOINT搭配指定ENTRYPOINT的默认参数
CMD [ 'param1', 'param2']
```
### ENTRYPOINT
ENTRYPOINT指令用于提供容器运行的默认命令，如果在docker run时指定了运行的命令，则CMD命令不会执行，有两种模式

```
# shell模式
ENTRYPOINT <command> ()

# exec模式
ENTRYPOINT [ "executable", "param1", "param2" ]
```
与CMD不同的是，ENTRYPOINT不会自动被docker run指定的命令覆盖，需要额外添加 --entrypoint 参数才可覆盖。
## 不常用指令
### VOLUME
用于向容器添加卷，可以提供共享存储等功能
```
VOLUME ['/data']
```
由于k8s提供了更加灵活的pv、pvc，所以很少使用此指令为容器挂载卷。
### USER
用于指定镜像以什么用户去运行
```
# 镜像以nginx身份运行，可以使用uid，gid等各种组合使用
USER nginx
```
实际工作中尚未使用过。
### ONBUILD
为镜像创建触发器，当一个镜像被用作其他镜像的基础镜像时，这个触发器会被执行。当子镜像被构建时会插入触发器中的指令。
```
ONBUILD <其他指令>
```
实际工作中尚未使用过。