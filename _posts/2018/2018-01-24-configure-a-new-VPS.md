---
layout: post    
title: Configure A New VPS    
category: Linux   
tags: [Ubuntu]
---

*Every time you have a new VPS, you need to download some software and configure the working environment, there is no question, this is not a pleasant thing. So I recorded the configuration process to facilitate configure VPS each time.*

## System version
Ubuntu 16.04 LTS   
kernel : 4.14   

## GCC
```
apt-get install build-essential
```
&nbsp;&nbsp;The easiest way to install. `build-essential` is a package which contains references to numerous packages needed for building software in general. `build-essential` contains tools (like the gcc compiler, make tool, etc) for compiling/building software from source.   
&nbsp;&nbsp;Actually, gcc 5.4.0, g++ 5.4.0 and make 4.1 will be installed by this way.   

## JAVA
### Download
JavaSE [Download Portal](http://www.oracle.com/technetwork/java/javase/downloads/index.html)   
&nbsp;&nbsp;First of all, It is necessary to download a jdk. The portal above is the link to the download page. Get the download link from the browser download list, then download jdk to VPS by using `wget` command. The following is an example, but it may not be available.   
```
wget http://download.oracle.com/otn-pub/java/jdk/8u162-b12/0da788060d494f5095bf8624735fa2f1/jdk-8u162-linux-x64.tar.gz?AuthParam=1516767328_ac989785c54226e7868cf9072b46fd7c
```
&nbsp;&nbsp;If the file name you download has garbled suffix, you need to remove the suffix after tar.gz. Then, you can extract the file.   
&nbsp;&nbsp;In order to extract .tar.gz file, you need to use the `tar zxvf filename.tar.gz` command.
```
mv jdk-8u162-linux-x64.tar.gz\?AuthParam\=1516767328_ac989785c54226e7868cf9072b46fd7c jdk-8u162-linux-x64.tar.gz
tar zxvf jdk-8u162-linux-x64.tar.gz
```
### Set environment variables
&nbsp;&nbsp;Then, setting environment variables is an essential step if you want to use Java.   
&nbsp;&nbsp;Open the `/etc/profile` file and add the following statement at the end.   
```
# JAVA
export JAVA_HOME=/usr/lib/java/jdk1.8.0_162
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:JAVA_HOME/lib:JRE_HOME/lib:${CLASSPATH}
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```
&nbsp;&nbsp;You need to replace `/usr/lib/java/jdk1.8.0_162` with your Java installation directory. And you can get the current path by use `pwd` command.     
&nbsp;&nbsp;After modify succeesfully, you need to use `source /etc/profile` to make the change take effect immediately.   
&nbsp;&nbsp;Now, you will see the version infomation if you use `java --version` command.   
### Additional info about environment variables
&nbsp;&nbsp;*There are several files records environment variables in Ubuntu system.*   
`/etc/environment` : System environment variables. This file doesn't support the commands, that is, you can not use export, but set the environment variables by Assignment statement, such as `path=/usr/lib`.   
`/etc/profile` : All user's environment variables. The file is read once when the user logs in.    
`/etc/bash.bashrc` : When the bash shell is opened, the file is read. That is, each time a new terminal shell is opened, the file is read.     
`~/.profile` : Single user's environment variables. The file is read once when the user logs in.  
`~/.bashrc` : The same to `/etc/bash.bashrc`, but valid for single user only.   

## Apache
```
apt-get install apache2
```
&nbsp;&nbsp;Execute the above instructions to install apache2.   
&nbsp;&nbsp;Use w3m test whether the installation is successful(You can install w3m by `apt-get install w3m` if you don't have one).   
&nbsp;&nbsp;If your local computer can not access the VPS, it may be caused by the close of 90 port. You can open 80 port by the following command if you use ipables.   
```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

## Tomcat
### Download
First of all, download is necessary. You can download binary package of tomcat 9.0.4 by the following command.
```
wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.4/bin/apache-tomcat-9.0.4-deployer.tar.gz
```
### Set environment variables
Extract file and set environment variables(The same process to Java).
```
# Tomcat
export CATALINA_HOME="/usr/lib/apache-tomcat-9.0.4"
export CLASSPATH=${CATALINA_HOME}/lib/servlet-api.jar:$CLASSPATH
```

## Vim configuration
Open the `/etc/vim/vimrc` file and add the following statement at the end.
```
set nu
set tabstop=4
set softtabstop=4
set ruler
set cindent
set shiftwidth=4
```
