---
layout: post
title: zabbix jmx
description: "zabbix jmx"
tags: [zabbix, devops]
image:
  background: triangular.png
---

# Zabbix JMX

* 官方文档： https://www.zabbix.com/documentation/3.4/manual/config/items/itemtypes/jmx_monitoring
* 注意点
  - 对于JMX, Zabbix server需要向[Zabbix Java Gateway](https://www.zabbix.com/documentation/3.4/manual/concepts/java)查询，而不是Zabbix Agent。
  - Java gateway 跟被监控应用的JMX management api的调用应该是畅通无阻的，网络上不应该被防火墙阻塞。如果JMX需要认证的话，是要能通过的。如果只是用在非生产环境，也可以取消认证。
  
~~~java
java \
  -Dcom.sun.management.jmxremote \
  -Dcom.sun.management.jmxremote.port=12345 \
  -Dcom.sun.management.jmxremote.authenticate=false \
  -Dcom.sun.management.jmxremote.ssl=false \
  -jar application.jar
~~~

# Zabbix Java Gateway Installation

* download rpm
  - zabbix-java-gateway-3.0.11-1.el7.x86_64.rpm from [Zabbix Official Repository](https://repo.zabbix.com/zabbix/)
  - java-1.8.0-openjdk-headless.x86_64 from os yum repo
* Install
  - Java gateway可以在zabbix server上安装，也可以在被监控节点上安装。不同的安装需要在zabbix server的配置上修改，见本文`Configuration` section。

~~~shell
yum -y install java-1.8.0-openjdk-headless.x86_64

rpm -ivh zabbix-java-gateway-3.0.11-1.el7.x86_64.rpm
~~~

## Configuration
* Java gateway 配置
  - 一般按照如下默认配置， /etc/zabbix/zabbix_java_gateway.conf

~~~shell
LISTEN_IP="0.0.0.0"
LISTEN_PORT=10052
PID_FILE="/var/run/zabbix/zabbix_java.pid"
START_POLLERS=5
TIMEOUT=3
~~~~

* Zabbix Server 配置 config /etc/zabbix/zabbix_server.conf
  - 如果java gateway安装在zabbix server上的话，默认配置就ok了
  - 如果java gateway安装在被监控节点上的话，JavaGateway的IP改成被监控机的外网IP

~~~shell
JavaGateway=127.0.0.1
JavaGatewayPort=10052 # java gateway listen port
StartJavaPollers=5    # java gateway threads, if value=0, means disable monitor java info)
~~~

* 配置完后重启一下服务

* jmx interface
  - 在zabbix的管理页面上添加上被监控节点的jmx interface，比如下图添加cassandra的jmx接口

![zabbix-passive-active](https://raw.githubusercontent.com/KasperDeng/kasperdeng.github.io/master/images/zabbix/zabbix-cassandra-jmx-interface.png)

* Template
  - 导入监控JMX的模板或自己创建，attach到被监控节点。一般模板对JMX的认证的用户名，密码是做成模板的环境变量，根据情况配置。

## 效果

![cassandra_dashboard](https://raw.githubusercontent.com/KasperDeng/kasperdeng.github.io/master/images/zabbix/cassandra_dashboard.png)


## 福利
* 安利一个zabbix模板的搜索
  - https://monitoringartist.github.io/zabbix-searcher/
  - github: https://github.com/monitoringartist/zabbix-community-repos
* 这里找到一个比较新的cassandra监控模板 [Template-Database-Cassandra-(m0t0k1ch1)](https://github.com/m0t0k1ch1/zabbix-cassandra-template)
