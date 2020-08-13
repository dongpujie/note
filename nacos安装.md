### 预备环境

1. 安装 JDK 1.8+
2. 安装 Maven 3.2.x+

```bash
# 下载   解压
tar -zxvf apache-maven-3.5.4-bin.tar.gz -C /usr/local/zdyg/
vim /etc/profile	# 添加环境变量
# 添加内容
# export MAVEN_HOME=/usr/local/zdyg/apache-maven-3.5.4
# export PATH=$MAVEN_HOME/bin/:$PATH
source /etc/profile
mvn -v	# 查看是否安装成功
```

3. 安装nacos

```shell
# 下载   解压
tar -zxvf apache-maven-3.5.4-bin.tar.gz -C /usr/local/zdyg/
```

4. 配置nacos使用mysql数据库

```shell
cd /usr/local/zdyg/nacos/conf/
vim application.properties

# 修改内容如下
### If user MySQL as datasource:
spring.datasource.platform=mysql

### Count of DB:
db.num=1

### Connect URL of DB:
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=zdyg
db.password=Zdyg0727!.

```



启动关闭

```shell
cd /usr/local/zdyg/nacos/bin/
# 启动
sh startup.sh -m standalone	# (standalone代表着单机模式运行，非集群模式)
# 关闭
sh shutdown.sh
```



线上nacos地址：`zdygu.cn:8848/nacos`

账号：nacos

密码：nacos

