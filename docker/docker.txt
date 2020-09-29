[TOC]

## docker 镜像

```shell
docker image ls
docker image save 
# 查看一个镜像的详细信息  包括他的历史信息
docker inspect 镜像名 
```



## docker 容器

容器:容器是一种轻量级,可移植,并将应用程序进行打包的技术, 使应用程序可以在拒户任何地方以相同的方式运行

![1565852157190](image/1565852157190.png)

#### 容器基本操作

![1565835517464](image/1565835517464.png)

```shell
# 创建容器 create   
docker create -t-i --name 指定容器名 镜像名

# 查看容器
docker ps -a

# 启动容器
docker start -a -i 容器名   # -a将容器的信息输出到当前终端上 -i保证在容器中的终端进行操作
# 暂停/继续容器
docker pause/unpause
# 停止容器
docker stop -t / docker kill  # -t指定关闭时间s 
# 删除容器
docker rm -f  (-f表示强制删除,容器在运行或暂停状态时)

# 创建并启动容器 run 
docker run 等价于 docker create + docker start -a  前台模式
docker run -d 等价于 docker create + docker start  后台模式
```

![1565836658318](image/1565836658318.png)

#### 容器的日志

![1565848978469](image/1565848978469.png)

#### 容器重命名

```
docker rename 容器 新名字
```



![1565849029459](image/1565849029459.png)

#### *容器运行时操作*

- 容器连接

  ```
  docker attach 
  --no-stdin  加上这个参数,在容器中输入的任何命令都不会被容器接收到,但是他的返回值会显示到当前终端上
  ```

- 容器中执行新命令

  ```
  docker exec  不影响容器的主进程
  ```

  

![1565849068593](image/1565849068593.png)

![1565850366710](image/1565850366710.png)

## 容器与镜像的关系

![1565852808839](image/1565852808839.png)

#### *容器提交 

```shell
docker commit   创建出一个可读可写的文件系统
```

![1565855103559](image/1565855103559.png)

#### 容器的导出\导出 (不常用)

```
导入
docker exprot 容器名称
导出
docker import 文件或服务器地址 起名
```

![1565854127799](image/1565854127799.png)

![1565854229237](image/1565854229237.png)

#### 容器与镜像的底层关系

![1565858437034](image/1565858437034.png)

镜像只读，容器是创建一个可读可写的文件系统

## docker网络管理

![1565915352431](image/1565915352431.png)

#### 查看网络

```shell
docker network ls
```

![1565915340466](image/1565915340466.png)

#### 创建网络

```shell
docker network create
```

![1565915482358](image/1565915482358.png)

