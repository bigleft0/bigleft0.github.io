---
layout: post
title:  "Hive搭建"
date:   2018-08-24 01:00:00
categories: Machine Learning
excerpt: Hive搭建
---

### Hive搭建


前提条件：
下载：https://mirrors.cnnic.cn/apache/hive/
安装了hadoop集群，

1．解压缩hive的软件包，使用命令：
tar -zxvf hive-3.1.0-bin.tar.gz  
2．进入hive的配置目录. 编辑/usr/local/data/hive-3.1.0/conf/hive-site.xml 
添加配置文件：
<?xml version="1.0" encoding="UTF-8" standalone="no"?>  
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>  
<configuration>  
   <property>  
        <name>javax.jdo.option.ConnectionURL</name>  
        <value>jdbc:mysql://linux1:3306/hive?createDatabaseIfNotExist=true</value>  
    </property>  
    <property>  
        <name>javax.jdo.option.ConnectionDriverName</name>  
        <value>com.mysql.jdbc.Driver</value>  
    </property>  
    <property>  
        <name>javax.jdo.option.ConnectionUserName</name>  
        <value>root</value>  
    </property>  
    <property>  
        <name>javax.jdo.option.ConnectionPassword</name>  
        <value>123456</value>  
    </property>  
    <property>    
   <name>hive.metastore.schema.verification</name>    
   <value>false</value>    
    <description>    
    Enforce metastore schema version consistency.    
    True: Verify that version information stored in metastore matches with one from Hive jars.  Also disable automatic    
          schema migration attempt. Users are required to manully migrate schema after Hive upgrade which ensures    
          proper metastore schema migration. (Default)    
    False: Warn if the version information stored in metastore doesn't match with one from in Hive jars.    
    </description>    
 </property>   
</configuration>

 hive-env.sh配置
export HADOOP_HOME=/usr/local/hadoop-2.8.4
export HIVE_HOME=/usr/local/data/hive-3.1.0
# Hive Configuration Directory can be controlled by:
export HIVE_CONF_DIR=/usr/local/data/hive-3.1.0/conf
# Folder containing extra libraries required for hive compilation/execution can be controlled by:
export HIVE_AUX_JARS_PATH=/usr/local/data/hive-3.1.0/lib
mysql驱动包导入
mysql驱动包放置到$HIVE_HOME\lib目录
/usr/local/data/hive-3.1.0/lib
记得文件要授权
chown -R mysql:mysql /usr/local
 对数据库进行初始化，执行命令：
  schematool   -initSchema  -dbType  mysql

报错1：
LF4J: Found binding in [jar:file:/usr/local/data/hive-3.1.0/lib/log4j-slf4j-impl-2.10.0.jar!/org/slf4j/impl/StaticLoggerBinder.class]
Jar包冲突
rm -f log4j-slf4j-impl-2.10.0.jar
报错2：
message from server: "Host 'linux1' is not allowed to connect to this MySQL server"
可能是帐号不允许从远程登陆，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"
登录数据库：mysql -u root -p

mysql>use mysql;
mysql>update user set host = '%' where user = 'root';
mysql>select host, user from user;
mysql>FLUSH   PRIVILEGES;

出现
Initialization script completed
schemaTool completed
启动成功
启动hive
进入到hive的bin目录执行命令：
hive



## 致谢

参考了：
https://www.cnblogs.com/mmzs/p/8079491.html

[我的作业]:https://github.com/smartjinyu/CS231n_assignments