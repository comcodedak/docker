#Version 1.5
# 正在下载基础镜像Debian
FROM hub.c.163.com/public/debian:7.9
MAINTAINER chengpanggang@126.com

# 更新系统源
RUN echo "deb http://mirrors.163.com/debian/ wheezy main non-free contrib" >>/etc/apt/sources.list
RUN echo "deb http://mirrors.163.com/debian/ wheezy-updates main non-free contrib" >>/etc/apt/sources.list
RUN echo "deb http://mirrors.163.com/debian/ wheezy-backports main non-free contrib" >>/etc/apt/sources.list
RUN echo "deb-src http://mirrors.163.com/debian/ wheezy main non-free contrib" >>/etc/apt/sources.list
RUN echo "deb-src http://mirrors.163.com/debian/ wheezy-updates main non-free contrib" >>/etc/apt/sources.list
RUN echo "deb-src http://mirrors.163.com/debian/ wheezy-backports main non-free contrib" >>/etc/apt/sources.list
RUN echo "deb http://mirrors.163.com/debian-security/ wheezy/updates main non-free contrib" >>/etc/apt/sources.list
RUN echo "deb-src http://mirrors.163.com/debian-security/ wheezy/updates main non-free contrib" >>/etc/apt/sources.list
RUN apt-get update


#安装ssh并设置远程root登录(暴露22端口)
RUN apt-get install -y openssh-server apt-utils
#替换字符串
RUN sed -i s/"PermitRootLogin without-password"/"PermitRootLogin yes"/g /etc/ssh/sshd_config
#暴露端口
EXPOSE 22

#安装vi
RUN apt-get install -y vim tar wget

#安装jdk  直接从宿主机拷贝
COPY jdk-7u60-linux-i586.tar.gz /root
RUN tar zxvf /root/jdk-7u60-linux-i586.tar.gz -C /root
RUN mv /root/jdk1.7.0_60 /root/jdk7
RUN echo "export JAVA_HOME =/root/jdk7" >> /etc/profile
RUN echo "export PATH = $JAVA_HOME/bin:$PATH" >> /etc/profile
RUN echo "export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar" >> /etc/profile
RUN rm -rf /root/jdk-7u60-linux-i586.tar.gz



#安装tomcat 下载
RUN wget http://archive.apache.org/dist/tomcat/tomcat-8/v8.5.9/bin/apache-tomcat-8.5.9.tar.gz
RUN tar zxvf apache-tomcat-8.5.9.tar.gz -C /root
RUN mv /root/apache-tomcat-8.5.9 /root/tomcat8
RUN chmod 777 /root/tomcat8/bin
RUN rm -rf apache-tomcat-8.5.9.tar.gz



#删除更新软件源信息
RUN rm -rf /var/lib/apt/lists/*
#暴露端口
EXPOSE 8080 
#设置容器开机启动项目 容器启动时  后台启动ssh 和tomcat
ENTRYPOINT  /usr/sbin/sshd -D


