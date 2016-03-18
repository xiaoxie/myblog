layout: post
title: 使用 docker 搭建开发环境
categories: docker
tags: docker
---

docker 想说爱你不容易，在此记录docker的实践过程 。

<!--more-->

安装配置
----

### 在Mac上通过安装Docker Toolbox 来安装docker服务

	# wget https://dn-dao-github-mirror.qbox.me/docker/toolbox/releases/download/v1.9.1c/DockerToolbox-1.9.1c.pkg


### 安装后可以是如下指令

![docker on macbook 1](/img/docker-on-macbook-1.png)

### 创建一个docker machine,并启动

	# docker-machine create mydev && docker-machine start mydev

### 查看docker machine 环境配置

	# docker-machine env mydev

### 根据提示设置指令环境

	# eval "$(docker-machine env mydev)"

使用docker来搭建一个svn服务
----

### 下载一个镜像

	# docker pull index.tenxcloud.com/tenxcloud/centos

### 下载bitnami-subversion

	# wget https://bitnami.com/redirect/to/78132/bitnami-subversion-1.8.13-2-linux-x64-installer.run

### 运行docker，挂在刚下载文件到容器中

	# docker run -v /mnt/soft/:/mnt/soft -it --name=bitsubversion index.tenxcloud.com/tenxcloud/centos

### 安装bitnami-subversion

	# chmod +x /mnt/soft/bitnami-subversion-1.8.13-2-linux-x64-installer.run && ./mnt/soft/bitnami-subversion-1.8.13-2-linux-x64-installer.run

### 提交更新保存到新的镜像

	# docker commit -m "added bitnami subversion" -a="xiaoxie" e98833fd6621 nextgo/subversion:v1

### 启动新的镜像

	# docker run \
	    --name=svn \
	    -v /mnt/svn/repository:/srv/repository \
	    -v /mnt/svn/conf:/srv/repository/conf \
	    -v /mnt/svn/logs:/opt/subversion-1.8.13-2/apache2/logs \
	    -p 8080:80 \
	    -it \
	    nextgo/subversion:v1 \

### 启动SVN服务

	sh /opt/subversion-1.8.13-2/ctlscript.sh start 

### 访问测试

	# curl http://127.0.0.1:8080



