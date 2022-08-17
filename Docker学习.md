# Docker学习

![image-20220803121552269](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803121552269.png)	





# Docker概述

## Docker为什么出现

![image-20220803122727613](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803122727613.png)	



## Docker的历史

![image-20220803140558921](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803140558921.png)



>  聊聊Docker	



​		Docker是基于Go开发的

​		官网;[Home - Docker](https://www.docker.com/)

![image-20220803141047407](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803141047407.png)	



## Docker能干嘛

> ###### 之前的虚拟机技术

![image-20220803141314039](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803141314039.png)





![image-20220803141442602](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803141442602.png)	



![image-20220803141621876](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803141621876.png)





# Docker的安装

## Docker的基本组成

![image-20220803141933667](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803141933667.png)









**镜像（image）**

​	docker镜像类似于一个模板，可以通过这个镜像来创建容器服务，tomcat===>  run  ===> tomcat01（提供服务器）

​	通过这个镜像可以创建多个容器（最终服务运行或者项目运行都是在这个容器中的）

**容器（container）**

​	独立运行一个或者一组应用，通过镜像创建的 

​	启动、停止、删除，基本命令

​	相当于建议的linux系统

​	

**仓库（repository）**

​	仓库是存放镜像的地方（共有、私有）	

​	默认是国外的，需要用镜像加速



## 安装Docker

> 环境准备



![image-20220803235023406](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220803235023406.png)





> 安装

```java
# 1、卸载原有的docker版本
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 2、需要的安装包
    yum install -y yum-utils
# 3、设置镜像的仓库
    yum-config-manager  --add-repo  http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 更新yum软件包索引
    [root@vm-8-11-centos /]# yum makecache fast

# 4、安装docker相关的   docker-ce 是社区版  ee是开发板
    yum install docker-ce docker-ce-cli containerd.io
  
# 5、启动docker
    systemctl start docker
    
    下图为测试docker的版本信息
```

![image-20220804002910443](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804002910443.png)	



docker安装成功

![image-20220804003105470](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804003105470.png)	



```she
# 8、查看一下下载的这个hello-world 镜像
	docker images
	
# 9、卸载docker
  （1）卸载依赖
  yum remove docker-ce docker-ce-cli containerd.io
  （2） 删除资源
  rm -rf /var/lib/docker(docker默认工作路径)
  
```





## 阿里云镜像加速

```java
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://axvfsf7e.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

```



## Run的流程和Docker原理	

![image-20220804015845825](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804015845825.png)	





## 底层原理

![image-20220804020348823](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804020348823.png)	









# Docker的基本命令

## 帮助命令

```shell
docker version     	# docker的版本信息
docker info			# docker的系统信息  镜像 容器的数量
docekr --help       # 帮助命令
```

帮助文档地址：[参考文档|Docker 文档](https://docs.docker.com/reference/)



## 镜像命令

```shell
[root@vm-8-11-centos /]# docker images   # 查看所有的镜像
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    feb5d9fea6a5   10 months ago   13.3kB
--------------------------------------------------------------
# 解释
repository ---> 镜像的仓库源
tag        ---> 镜像标签
image id   ---> 镜像ID
created    ---> 创建时间
size       ---> 镜像大小

# 可选项
-a           # 列出所有镜像
-q 			 # 只显示镜像的id

```

**docker search 搜索镜像**

![image-20220804100915363](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804100915363.png)	

![image-20220804105446578](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804105446578.png)	

![image-20220804105509791](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804105509791.png)	![](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804105756028.png)	

## 容器命令



**我们有了镜像才可以创建容器，linux，下一个centos镜像来测试学习**

```shell
docker pull centos 			(最新版本)
```

创建新容器并启动

![image-20220804112328279](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804112328279.png)		 



 ![image-20220804130055729](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804130055729.png)	





![image-20220804130804528](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804130804528.png)	



## 常用其他命令



 ![image-20220804134431520](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804134431520.png)	 ![image-20220804134841736](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804134841736.png)	

![image-20220804134923938](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804134923938.png)	

![image-20220804140120890](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804140120890.png)	



![image-20220804140411471](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804140411471.png)	



**从容器内拷贝文件到主机上**

![image-20220804140749362](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804140749362.png)	







## 小结

![image-20220804141906087](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804141906087.png)	





![image-20220804141952700](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804141952700.png)	

​	![image-20220804142020859](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220804142020859.png)	







## 作业练习

> Docker安装Nginx

```shell
# 搜索镜像 search 
# 下载镜像 pull
# 运行测试
[root@vm-8-11-centos ~]# docker images   # 查看镜像
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
nginx        latest    605c77e624dd   7 months ago    141MB
centos       latest    5d0da3dc9764   10 months ago   231MB
[root@vm-8-11-centos ~]# docker run -d --name naginx01 -p:3344:80 nginx 
	# 以后台方式启动  name为naginx01  公网3344端口访问容器的80端口
dd4affcd4988c481979fce69ba22e9f345dc8a88031f04938af692ba98eae172
[root@vm-8-11-centos ~]# docekr ps
-bash: docekr: command not found
[root@vm-8-11-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
dd4affcd4988   nginx     "/docker-entrypoint.…"   14 seconds ago   Up 13 seconds   0.0.0.0:3344->80/tcp, :::3344->80/tcp   naginx01
2b4522a07fbc   centos    "/bin/bash"              17 hours ago     Up 17 hours                                             festive_mclean
[root@vm-8-11-centos ~]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>


```

![image-20220805074353477](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805074353477.png)	

**端口暴露**

![image-20220805060622080](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805060622080.png)	













![image-20220805081440006](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805081440006.png)	![image-20220805094309934](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805094309934.png)	![image-20220805094432933](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805094432933.png)



![image-20220805084406214](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805084406214.png)	



![image-20220805084553478](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805084553478.png)	





![image-20220805084814649](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805084814649.png)	





![image-20220805085018651](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805085018651.png)	





## 可视化





![image-20220805090335708](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805090335708.png)	





```SHELL
docker run -d -p 8088:9000 \
--restart=always -v/var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```



# Docker镜像讲解

## 镜像是什么

![image-20220805092048031](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805092048031.png)	

## Docker镜像加载原理

![image-20220805092339954](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805092339954.png)	

![image-20220805092940379](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805092940379.png)		

## 分层理解

![image-20220805093829194](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220805093829194.png)		







