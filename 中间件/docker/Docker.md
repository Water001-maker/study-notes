# Doker

## docker的思想：

> 集装箱:镜像
>
> 码头: 中央仓库
>
> 容器：镜像启动后成为容器，会在当前Liunx中开辟一片空间，虚拟出一个小型的linux服务

## docker的安装：

```
yum -y install yum-utils device-mapper-persistent-data lvm2  #下载环境
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo #指定阿里云的镜像源
yum makecache fast #快速建立缓存
yum -y install docker-ce #开始安装docker
systemctl start docker #启动docker
docker run hello-world #测试docker是否安装成功
```

## 镜像的命令

```
docker images #查看本地的所有的镜像
docker rmi 镜像id #删除镜像，注意如果镜像已经被启动为了容器，首先关闭容器，将容器删除，再来删除镜像
docker search 名称 #从中央仓库搜索镜像
docker pull 名称[:tag]  #从中央仓库下载镜像到本地
docker save -o 路径/镜像名称 镜像id #将docker中的镜像导入到本地的文件
docker load -i 镜像名称 #将本地的镜像文件导入到docker中作为镜像
docker tag 镜像id 镜像名称:版本 #重新命名镜像文件
```

## 容器的命令

```
docker run --name 容器的名称  镜像id #将镜像运行为容器
-d #代表后台运行 
-p #代表 端口号的映射，将宿主机端口号与容器端口号进行映射

#docker run -d -p 8080:8080 --name 容器的名称  镜像id 

docker ps -a #查看正在运行或者停止的容器
docker logs -f 容器id #查看容器的日志
docker exec -it 容器id bash  #进入到容器内部
docker stop|restart|start|rm 容器id #停止，重启，启动，删除容器
```

```
进入容器
docker exec -it 0411d1108ed4 /bin/bash
运行容器
docker run -d -p 8080:8080 --name 容器的名称  镜像id 
```

## tomcat安装

```
docker pull daocloud.io/library/tomcat:8.5.15-jre8 #下载镜像
docker run -d -p 8080:8080 --name 容器名称 镜像id #启动tomcat

```

## mysql安装

```
docker pull mysql:5.7.4 #下载mysql镜像
docker run -d -p 33306:3306 -e MYSQL_ROOT_PASSWORD=root --name mysql aa5364eb3d85
#注意 防火墙必须关闭 如果是云服务，需要在安全组中配置当前宿主机的端口号的开放
```

## 数据卷的命令

```
1.docker volume create 数据卷的名称 #默认路径存在放/var/lib/docker/volumes/数据卷名称/_data
2.docker volume inspect 数据卷名称 #查看数据卷的详情信息，比如创建 时间，存放的路径...
3.docker volume ls #查看所有的数据卷
4.docker volume rm 数据卷名称 #删除数据卷
5.
方法一（创建一个数据卷）：docker run -d -p 宿主机端口号:容器端口号 -v 数据卷名称：宿主机的路径/容器内部的路径 --name 镜像id

方法二（直接只用主机路径）：docker run -d -p 宿主机端口号:容器端口号 -v 宿主机的路径/容器内部的路径 --name 镜像id
```

## Dockerfile的命令

> 自定义镜像内容
>
> from  基于那个镜像
>
> copy  文件夹  镜像内部的路径
>
> ![image-20220309203516742](Docker.assets\image-20220309203516742.png)

```
docker build -t 镜像名称:版本 .
通过docker images 查看刚刚创建的镜像内容
```

一定要和dockerfile在同级目录下！！！

![image-20220309204502154](Docker.assets\image-20220309204502154.png)

## Docker-compose 命令

> 批量管理容器，通过docker-compose.yml文件来进行管理
>
> 1.安装docker-compose
>
> ​	1.1 将提供的docker-compose-Linux-x86_64文件上传到linux中
>
> ​	1.2 将-docker-compose-Linux-x86_64改名  mv docker-compose-Linux-x86_64 docker-compose   
>
>  ​     1.3 chmod 777 docker-compose    #赋予权限
>
> ​	 1.4 mv docker-compose /usr/local/bin
>
> ​	 1.5 vi /etc/profile
>
> ​	 1.6 `export PATH=$JAVA_HOME/bin:/usr/local/bin:$PATH`
>
> ​	 1.7 source /etc/profile   #让修改的属性生效
>
> ​	 1.8 docker-compose

1.编写docker-compose.yml文件

```yml
version: '3.1'
services:
    mysql:
        restart: always #跟随docker启动而启动
        image: daocloud.io/library/mysql:5.7.4  #依赖于那个镜像
        container_name: mysql   #容器的名称
        ports:
            - 33306:3306  #端口号的映射 宿主机端口号：容器的端口号
        environment:
            MYSQL_ROOT_PASSWORD: root  #管理员的账号
            TZ: Asia/Shanghai          #指定时区
        volumes:
            - /opt/docker_mysql_tomcat/mysql_data:/var/lib/mysql  #数据卷映射
    tomcat:
        restart: always
        image: daocloud.io/library/tomcat:8.5.15-jre8
        container_name: rose
        ports:
            - 8999:8080
        environment:
             TZ: Asia/Shanghai   
        volumes:
            - /opt/docker_mysql_tomcat/tomcat_data:/usr/local/tomcat/webapps
            - /opt/docker_mysql_tomcat/tomcat_log:/usr/local/tomcat/logs
```

2.将yml文件上传到Linux中

3.在当前yml文件所在路径中输入命令

```
1.docker-compose up -d #批量启动容器
2.docker-compose down #批量停止删除容器
3.docker-compose start|stop|restart #启动停止重启 所有的容器
4.docker-compose logs -f #查看日志
5.docker-compose ps #查看docker-compose管理的容器
```
