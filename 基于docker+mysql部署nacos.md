# 基于docker+mysql部署nacos

## 1、安装docker

## 2、安装mysql

## 3、安装nacos

##### 拉取镜像

```
docker pull nacos/nacos-server
```

##### 创建数据库

在数据库中创建naocs数据库或者导入nacos数据库



##### 运行容器

###### 方式一、直接部署

```shell
#查看mysql的docker的虚拟ip mysql为MySQL容器名
docker inspect --format='{{.NetworkSettings.IPAddress}}' mysql
#运行容器
docker run -d \
-e MODE=standalone \
-e PREFER_HOST_MODE=hostname \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=172.17.0.3 \
-e MYSQL_SERVICE_PORT=3306 \
-e MYSQL_SERVICE_USER=root \
-e MYSQL_SERVICE_PASSWORD=root \
-e MYSQL_SERVICE_DB_NAME=nacos \
-p 8848:8848 \
--name nacos \
--restart=always \
nacos/nacos-server:1.4.0
```

###### 参数说明：

```yaml
# 单节点模式
MODE=standalone
# 数据库地址
MYSQL_SERVICE_HOST
# 数据库用户名
MYSQL_SERVICE_USER
# 数据库密码
MYSQL_SERVICE_PASSWORD
# 需连接的数据库名称
MYSQL_SERVICE_DB_NAME
# 端口映射
-p 8848:8848 
# 任意时候重启容器，开机就能自动启动容器（需设置docker为开机自启）
--restart=always
```

###### 方式二、指定配置文件

若需要映射容器内文件到宿主机上，或需要配置其它属性，可以先按方式1启动然后拷贝出配置文件，修改后启动

```
#拷贝配置文件
docker cp nacos /home/nacos/conf/application.properties /home/docker/nacos/conf
#拷贝logback日志配置文件
docker cp nacos /home/nacos/conf/nacos-logback.xml /home/docker/nacos/conf
```

修改application.properties配置文件如下

```properties
# spring
server.servlet.contextPath=/nacos
server.contextPath=/nacos
server.port=8848
spring.datasource.platform=mysql
nacos.cmdb.dumpTaskInterval=3600
nacos.cmdb.eventTaskInterval=10
nacos.cmdb.labelTaskInterval=300
nacos.cmdb.loadDataAtStart=false
db.num=1
db.url.0=jdbc:mysql://192.168.56.10:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&serverTimezone=Asia/Shanghai
db.url.1=jdbc:mysql://192.168.56.10:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&serverTimezone=Asia/Shanghai
db.user=root
db.password=root
### The auth system to use, currently only 'nacos' is supported:
nacos.core.auth.system.type=nacos


### The token expiration in seconds:
nacos.core.auth.default.token.expire.seconds=18000

### The default token:
nacos.core.auth.default.token.secret.key=SecretKey012345678901234567890123456789012345678901234567890123456789

### Turn on/off caching of auth information. By turning on this switch, the update of auth information would have a 15 seconds delay.
nacos.core.auth.caching.enabled=false
```

```
#停止并删除刚才创建的容器
docker stop nacos
docker rm nacos
#重新运行容器
docker run -d \
-e MODE=standalone \
-p 8848:8848 \
-v /home/docker/nacos/conf:/home/nacos/conf \
-v /home/docker/nacos/logs:/home/nacos/logs \
-v /home/docker/nacos/data:/home/nacos/data \
--name nacos-mysql \
--restart=always \
nacos/nacos-server

#最后测试访问
```

