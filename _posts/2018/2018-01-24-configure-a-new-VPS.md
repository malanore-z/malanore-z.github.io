---
layout: post    
title: Configure A New VPS    
category: Linux   
tags: [Ubuntu]
---

*每次购买新的VPS之后都要安装诸多工具， 因此将其记录下来， 方便忘记指令的时候查阅。*

## GCC
```
apt-get install build-essential
```
最简单的办法， 会给安装gcc和g++ 5.4.0， make 4.1

## JAVA
JavaSE [Download](http://www.oracle.com/technetwork/java/javase/downloads/index.html)   
首先下载jdk(jdk8_162)   
```
wget http://download.oracle.com/otn-pub/java/jdk/8u162-b12/0da788060d494f5095bf8624735fa2f1/jdk-8u162-linux-x64.tar.gz?AuthParam=1516767328_ac989785c54226e7868cf9072b46fd7c
```
解压
```
mv jdk-8u162-linux-x64.tar.gz\?AuthParam\=1516767328_ac989785c54226e7868cf9072b46fd7c jdk-8u162-linux-x64.tar.gz
tar zxvf jdk-8u162-linux-x64.tar.gz
```
然后设置环境变量   
/etc/environment : 系统的环境变量。 此文件不能包含命令， 即不能使用export， 而是通过赋值的方式设置环境变量， e.g. path=/usr/lib   
/etc/profile ： 所有用户的环境变量。 当用户登录时该文件读取一次。 修改完后使用`source /etc/profile`使修改即时生效。  
/etc/bash.bashrc : 当 bash shell 被打开时，该文件被读取。也就是说，每次新打开一个终端 shell，该文件就会被读取。     
~/.profile : 单个用户的环境变量， 当用户登录时该文件读取一次。  
~/.bashrc : 只对单个用户生效，当登录以及每次打开新的 shell 时，该文件被读取。   

在`/etc/profile`文件最后添加如下环境变量。 ‘pwd’指令可以获取当前绝对路径。   
```
# JAVA
export JAVA_HOME=/usr/lib/java/jdk1.8.0_162
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:JAVA_HOME/lib:JRE_HOME/lib:${CLASSPATH}
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```
别忘了执行`source /etc/profile`使添加的环境变量即时生效。   
之后执行`java -version`若能看到版本信息则环境变量设置成功。

## Apache
```
apt-get install apache2
```
执行以上指令即可
使用w3m测试是否安装成功， 若没有w3m， 通过`apt-get install w3m`安装。   
执行`w3m localhost`， 如果能看到apache的初始页， 则说明安装成功   
此时本地电脑不一定能访问到VPS的80端口， 需要配置防火墙规则。   
iptables添加80端口
```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```
此时在浏览器中输入VPS的地址应当能看到这一页

## Tomcat
首先下载tomcat二进制包(下方的下载地址是tomcat9.0.4)
```
wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.4/bin/apache-tomcat-9.0.4-deployer.tar.gz
```
解压之后配置如下环境变量(解压和配置环境变量过程同java)
```
# Tomcat
export CATALINA_HOME="/usr/lib/apache-tomcat-9.0.4"
export CLASSPATH=${CATALINA_HOME}/lib/servlet-api.jar:$CLASSPATH
```

## Vim configuration
在`/etc/vim/vimrc`文件的最后添加如下语句
```
set nu
set tabstop=4
set softtabstop=4
set ruler
set cindent
set shiftwidth=4
```
