### 内核升级

#### centos7.0升级内核

查看系统内核版本

```shell
# uname -r
3.10.0-514.el7.x86_64
# cat /etc/redhat-release 
CentOS Linux release 7.3.1611 (Core)
```

升级内核

```shell
rpm -import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

> 报错
>
> curl: (35) Peer reports incompatible or unsupported protocol version.
>
> curl 不兼容或不支持的协议版本
>
> 解决
>
> yum update -y nss curl libcurl

查看可用内核包

```shell
yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
```

安装最新的主线稳定内核，使用如下命令：(以后这台机器升级内核直接运行这句就可升级为最新稳定版)

```shell
yum -y --enablerepo=elrepo-kernel install kernel-ml.x86_64
```

查看默认启动顺序：

```shell
awk -F\' '$1=="menuentry " {print $2}' /etc/grub2.cfg
```

修改内核的启动顺序为0：

```shell
vim /etc/default/grub

# 修改为0
GRUB_DEFAULT=0	
```

重新创建内核配置，如下：

```shell
grub2-mkconfig -o /boot/grub2/grub.cfg
```

重启

reboot

> 报错
>
> ```
> error: /boot/vmlinuz-4.18.12-041812-generic has invalid signature
> error: you need to load the kernel first
> ```
>
> 解决
>
> BIOS界面关闭secure BOOT

删除旧内核

```shell
# 1.验证
uname -r

2.删除旧内核
# 查看
rpm -qa | grep kernel
kernel-headers-3.10.0-957.12.2.el7.x86_64
kernel-tools-3.10.0-957.12.2.el7.x86_64
kernel-3.10.0-957.12.2.el7.x86_64
kernel-3.10.0-957.el7.x86_64
kernel-devel-3.10.0-957.12.2.el7.x86_64
kernel-tools-libs-3.10.0-957.12.2.el7.x86_64

# 删除
yum remove  xxx xxx xxx  xxx 
```



### 安装docker

官网：https://docs.docker.com/engine/install/centos/#os-requirements

#### 设置存储库

安装`yum-utils`软件包（提供`yum-config-manager` 实用程序）并设置**稳定的**存储库。

```shell
$ sudo yum install -y yum-utils

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

> （可选）启用边缘和测试存储库，通常是禁用状态，也可以选择启用
>
> ```shell
> $ sudo yum-config-manager --enable docker-ce-nightly	# 边缘存储库
> $ sudo yum-config-manager --enable docker-ce-test	# 测试存储库
> ```
>
> 您可以通过运行带有标志的命令来禁用**边缘**或**测试**存储库 。要重新启用它，请使用该标志。以下命令禁用**边缘**存储库。`yum-config-manager --disable --enable`
>
> ```shell
> $ sudo yum-config-manager --disable docker-ce-nightly
> ```
>
> -----------------------------------------------------------------------------------------------------------------------------------------
>
> 如果安装了python3，并且将python指向python3， 会有很多文件不能运行， 将文件第一行python改为python2

#### 安装

安装*最新版本*的Docker Engine和容器，或转到下一步以安装特定版本：

```
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

> 列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序，并被截断：
>
> ```shell
> $ yum list docker-ce --showduplicates | sort -r
> ```
>
> 通过其完全合格的软件包名称安装特定版本，该软件包名称是软件包名称（`docker-ce`）加上版本字符串（第二列），从第一个冒号（`:`）一直到第一个连字符，并用连字符（`-`）分隔。例如，`docker-ce-18.09.1`。
>
> ```shell
> $ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
> ```

启动Docker

```shell
$ sudo systemctl start docker
```


通过运行hello-world 映像来验证是否正确安装了Docker Engine 。

```shell
$ sudo docker run hello-world
```

### 安装crawlab

使用docker-compose

```shell
pip install docker-compose
```

在`docker-compose.yml`文件目录下使用`docker-compose ps`验证是否安装正常

`docker-compose.yml` 文件内容

```python
version: '3.3'  # Docker Compose 的版本号（请看后续说明）
services:  # 服务
  master:  # 服务名称
    image: tikazyq/crawlab:latest  # 服务对应的镜像名称
    container_name: master  # 服务对应的容器名称
    environment:  # 这里定义传入的环境变量
      CRAWLAB_SERVER_MASTER: "Y"  # 是否为主节点，Y/N
      CRAWLAB_MONGO_HOST: "mongo"  # MongoDB host，由于在 Docker Compose 里，可以引用服务名称
      #CRAWLAB_REDIS_ADDRESS: "redis"  # Redis host，由于在 Docker Compose 里，可以引用服务名称
      CRAWLAB_REDIS_ADDRESS: "192.168.0.240"  # Redis host，由于在 Docker Compose 里，可以引用服务名称
      CRAWLAB_REDIS_PASSWORD: ""
      #CRAWLAB_REDIS_DATABASE: "11"
      CRAWLAB_REDIS_PORT: "6379"
    ports:  # 映射的端口
      - "9999:9999" # 前端端口
    depends_on: # 依赖的服务
      - mongo  # MongoDB
      - chrome
      #- redis  # Redis
  worker:  # 工作节点，与主节点配置类似，不重复写了
    image: tikazyq/crawlab:latest
    container_name: worker
    environment:
      CRAWLAB_SERVER_MASTER: "N"
      CRAWLAB_MONGO_HOST: "mongo"
      CRAWLAB_REDIS_ADDRESS: "192.168.0.240"
      #CRAWLAB_REDIS_ADDRESS: "redis"
      CRAWLAB_REDIS_PASSWORD: ""
      #CRAWLAB_REDIS_DATABASE: "11"
      CRAWLAB_REDIS_PORT: "6379"
    depends_on:
      - mongo
      - chrome
      #- redis
  mongo:  # MongoDB 服务名称
    image: mongo:latest  # MongoDB 镜像名称
    restart: always  # 重启策略为“总是”
    ports:  # 映射端口
      - "27017:27017"
#  redis:  # Redis 服务名称
#    image: redis:latest  # Redis 镜像名称
#    restart: always  # 重启策略为“总是”
#    ports:  # 映射端口
#      - "6378:6378"
  chrome:
    image: selenium/standalone-chrome:latest
    ports:
      - "4444:4444"
    shm_size: 1g
        
```

`docker-compose up -d` 启动 crawlab

