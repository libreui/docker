### 1、准备工作
先在主机创建工作文件夹，为了放置 Tomcat 的配置文件等。创建文件夹的方法，自己搞定。

```
[root@dockeruat tomcat]# pwd
/usr/local/tomcat
 
[root@dockeruat tomcat]# ls
bin  conf  docker-compose.yml  logs  webapps
```

先随便启动一个 tomcat 容器（用第一种方法，docker run），主要是为了获取 tomcat 容器内部的配置文件。

```
#启动一个容器
docker run -d --name tomcat tomcat
```

```
# 查看 容器 获取容器ID
docker ps -a
```

启动容器后，容器内部会生成 tomcat 的配置文件，将其复制到本地对应文件夹内（上面自己创建好的文件夹内）。进入容器，查看 tomcat 文件夹内的内容，进入容器命令如下。

```
docker exec -it tomcat bash
# 容器内命令
cd /usr/local/tomcat
```

其中将，容器内 tomcat 文件夹下的 conf bin logs webapps 里面的内容都拷贝到上面宿主机上创建的对应文件夹内。容器的文件拷贝到宿主机的命令如下：

```
# 注意!是在宿主机上执行这条命令。
docker cp 容器名:/usr/local/tomcat/webapps/* /usr/local/tomcat/webapps
其它文件夹内的内容也要这样拷贝到宿主机对应的文件夹内。
```

之后，这个临时创建的容器可以删除了。删除容器命令：

```
docker rm -f tomcat
```

### 2、部署容器

开始部署 tomcat 容器，就是上面提到的用第三种方式部署。

创建这个文件，docker-compose.yml 黏贴以下内容。文件放置位置，看最上面的目录结构。

```yml
version: '3'
services:
  tomcat:
    user: root
    restart: always
    container_name: tomcat
    image: tomcat
    privileged: true
    environment:
      - TZ="Asia/Shanghai"
    ports:
      - 1002:8080
    volumes:
      - /usr/local/tomcat/webapps/:/usr/local/tomcat/webapps/
      - /usr/local/tomcat/conf:/usr/local/tomcat/conf
      - /usr/local/tomcat/logs:/usr/local/tomcat/logs
      - /usr/local/tomcat/bin:/usr/local/tomcat/bin
      - /etc/localtime:/etc/localtime
```

启动容器

```
docker-compose up -d
```

### 3、总结
直接用 docker-compose.yml 方式创建容器，无法启动 tomcat ，因为缺少启动文件，所以先临时创建一个容器，然后把容器内需要的文件复制到宿主机，再用 docker-compose.yml 方式挂载好数据券目录创建正式的容器。