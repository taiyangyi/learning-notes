# 帮助命令

```
docker version

docker info

docker --help
```

# 镜像命令

## 1. 列出本机主机上的镜像

```
docker images [OPTIONS] 镜像名字[:TAG]
```

### OPTIONS说明

```java
-a 列出本地所有的镜像（含中间映像层） docker images -a
-q 只显示镜像ID
--digests 显示镜像的摘要信息
--no-trunc 显示完整的镜像信息
```

### 参数说明

REPOSITORY：表示镜像的仓库源

TAG：镜像的标签

IMAGE ID：镜像ID

CREATED：镜像创建时间

SIZE：镜像大小

同一个仓库源可以有多个TAG，代表这个仓库源的不同个版本，我们使用REPOSITORY:TAG来定义不同的镜像。

## 2. 从Docker Hub查找镜像

访问地址：`https://hub.docker.com`

```
docker search [OPTIONS] 镜像名字

```

### OPTIONS说明

```java
--no-trunc 显示完整的镜像描述
-s 列出收藏数不少于指定值的镜像
--automated 只列出automated build类型的镜像
```

### 参数说明

REPOSITORY：表示镜像的仓库源

TAG：镜像的标签

IMAGE ID：镜像ID

CREATED：镜像创建时间

SIZE：镜像大小

## 3. 从镜像仓库中拉取或者更新指定镜像

```
docker pull [OPTIONS] 镜像名字[:TAG]
```

### OPTIONS说明

```java
-a 拉取所有 tagged 镜像
--disable-content-trust 忽略镜像的校验,默认开启
```

## 4. 删除本地一个或多少镜像

```
docker rmi [OPTIONS] 镜像ID 删除单个

docker rmi [OPTIONS] 镜像1:TAG 镜像名2:TAG 删除多个

docker rmi [OPTIONS] $(docker images -qa) 删除全部
```

### OPTIONS说明

```java
-f 强制删除
--no-prune 不移除该镜像的过程镜像，默认移除
```

# 容器命令

前提说明：有镜像才能创建容器，以Centos镜像为例 docker pull centos