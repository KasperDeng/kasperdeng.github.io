---
layout: post
title: zabbix snmp trap
description: "SNMP traps in zabbix"
tags: [zabbix, devops]
image:
  background: triangular.png
---

# SNMP #

* SNMP - Simple Network Management Protocol
  - TCP/IP协议族的一部分
  - 能够使网络设备之间能够方便地交换管理信息
  - 能够让网络管理员管理网络的性能，发现和解决网络问题及进行网络的扩充
* SNMP管理设备有查询(query)和上报(trap)两种方式
* SNMP TRAP
  - SNMP中标准的上报机制

# Zabbix的SNMP TRAP #
* zabbix既可以**查询**SNMP agent设备，也支持设备**上报**SNMP trap, 本文主讲`SNMP Trap`
* zabbix对于SNMP trap的支持，是需要依赖外部的工具，在Liunx上，主要就是`SNMPtrapd`和`SNMPtt`
  - SNMPTrapd - 是一个用于接收外围snmp agent设备上报的SNMP Trap的一个守护进程
  - SNMPtt - Trap Translator， 简单说就是用于翻译或格式化SNMP Trap的，最后将结果写到文件。

* 消息处理流程
![snmp-trap-2-zabbix](http://www.plantuml.com/plantuml/png/RKwz3e9039xjKpGuym8C9ZeO1pKcEBaLQU2IetSvgl3u7g90Z9sQxzzJK7CtdaGnbyPlAnQ5mlMOtAWeJ8yvXoS7FBXM4rmVJLNhpZuOLdvXRQDL0_do3wo0drjIrwZU6yj_oR5wR0QM4kCWW6bVkfaBCypFfmiEFqmWfb-9PdWdQx4cAMqC6y4t_rsG3MPfBIdFMHZrs0KJCUigM0xwCJTEZBg0XCBT7F02)

* 安装：依赖包
   - 这里以RedHat 7/CentOS 7为例子，记录需要用到的工具以及相关的依赖包（以下有链接的一般表示在系统默认的软件包管理器里面缺失的，需要搜索下载安装）

```shell
net-snmp.x86_64 (for snmptrapd)

[snmptt](http://rpm.pbone.net/index.php3/stat/4/idpl/27126799/dir/redhat_el_7/com/snmptt-1.4-0.9.beta2.el7.noarch.rpm.html)
   \--- [perl-Config-IniFiles](http://rpm.pbone.net/index.php3/stat/4/idpl/29944336/dir/redhat_el_7/com/perl-Config-IniFiles-2.83-4.1.noarch.rpm.html)
        \--- perl-List-MoreUtils.x86_64
   \--- [perl-Net-SNMP](http://rpm.pbone.net/index.php3/stat/4/idpl/6028905/dir/redhat_7.x/com/perl-Net-SNMP-v5.0.1-1.pp-rh73.noarch.rpm.html)
        \--- perl-Digest-HMAC.noarch
        \--- perl-Digest-MD5.x86_64
        \--- perl-Digest-SHA.x86_64
        \--- perl-Digest-SHA1.x86_64

net-snmp-utils.x86_64 (for snmptrap)
```

# 配置snmptrapd #
* `/etc/snmp/snmptrapd.conf`
  - 配置snmptrapd默认的handler是snmptt, e.g. `traphandle default /usr/sbin/snmptt`
  - 配置是否禁止做身份验证等
* 检查snmptrapd启动服务(e.g. `/usr/lib/systemd/system/snmptrapd.service`)所带的参数是否满足需求, 默认安装后的启动参数比较弱爆，建议添加以下的参数
  - `-Lf [Log file]`: Log messages to the specified file. snmptrapd的日志文件，用于检查或记录是否收到对应的SNMP trap
  - REF: http://net-snmp.sourceforge.net/docs/man/snmptrapd.html
* 添加snaptrapd作为默认启动服务，跟zabbix server service的behavior保持一致
  - REHL 6.x `chkconfig snmptrapd on`
  - REHL/CentOS 7.x `systemctl enable snmptrapd`

# 配置snmptt #
* `/etc/snmp/snmptt.ini`
  - 配置snmptt程序
    + log_file = /tmp/my_zabbix_traps.tmp  // 翻译的SNMP trap写到的文件路径, 一定要跟Zabbix Server的配置`SNMPTrapperFile`一致
    + date_time_format = %H:%M:%S %Y/%m/%d
    + 使用snmptt的standalone model

```shell
# Set to either 'standalone' or 'daemon'
# standalone: snmptt called from snmptrapd.conf
# daemon: snmptrapd.conf calls snmptthandler
```

* `/etc/snmp/snmptt.conf`
  - 配置如何翻译由snmptrapd扔过来的snmp trap
  - 跟具体设备上报业务相关
    + 主要配置你监测的设备所产生的SNMP trap的事件(事件的ID, 对应的OID，事件名称，状态等)
    + 配置要翻译成/格式化的格式
  - 对于Zabbix
    + 记得在`FORMAT`关键字后添加`ZBXTRAP $aA`，e.g. `FORMAT ZBXTRAP $aA`，表示从某个host (这里是IP/DNS name, 需要对应监控的机器的SNMP interface上配置的IP/DNS name) 上收到了一个Zabbix需要关注的SNMP trap
    + Zabbix server就是通过扫TRAP文件中新的TRAP中是否带这些关键字从而再去SNMP trap item里面去匹配的。

# 配置 Zabbix #
* 修改`/etc/zabbix/zabbix_server.conf`
   - StartSNMPTrapper = 1  // 让zabbix支持接收SNMP trap
   - SNMPTrapperFile= /tmp/my_zabbix_traps.tmp // 指定zabbix去扫的SNMP Trap文件，也就是SNMPtt翻译后所写的文件
* 重启zabbix server: `systemctl restart zabbix-server`

# 配置zabbix的SNMP Trap Template
* SNMP Trap Item
  - 选type为snmp trap，设置key和log time format
    + Key: snmptrap["Status Events"]: 这里是正则匹配的(snmptrap[<regex>])，匹配的是SNMP trap的事件的名称
    + Log time format: hh:mm:ss yyyy/MM/dd
  - 当zabbix server收到相关的SNMP trap，并能匹配到对应的item的时候。可以在监控页面的`latest data`看回这个item的详细历史记录
- SNMP Trap Triggers
  - 可以配置产生trigger的条件，根据SNMP trap item的统计信息来做条件。
  - 可以配置trigger恢复(问题解决)的条件

# 经验总结 #
* 注意串通整个流程过程中各个组件的日志。因为SNMP trap on Zabbix中间流经了好几个组件，任何一个出问题，都会导致SNMP trap没法在Zabbix上被监测到。

# 坑 #
* SELINUX的安全问题
  - 如果打开了SELINUX，很可能会导致文件的访问权限问题。要时刻关注下`/var/log/audit/audit.log`
    + 比如SNMPtt写的log文件，Zabbix Server没权限读取，尽管把文件的权限全打开。
  - 如果发现SELINUX所block的问题，需要用到`audit2allow`去解决，参照以下例子。
  - REF: https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Troubleshooting-Fixing_Problems.html#sect-Security-Enhanced_Linux-Fixing_Problems-Allowing_Access_audit2allow

```shell
shell>tail -f /var/log/audit/audit.log
type=SYSCALL msg=audit(1478754989.834:4148): arch=c000003e syscall=2 success=no exit=-13 a0=25de540 a1=0 a2=7ffe07dedda0 a3=0 items=0 ppid=15596 pid=15626 auid=4294967295 uid=995 gid=992 euid=995 suid=995 fsuid=995 egid=992 sgid=992 fsgid=992 tty=(none) ses=4294967295 comm="zabbix_server" exe="/usr/sbin/zabbix_server_mysql" subj=system_u:system_r:zabbix_t:s0 key=(null)
type=AVC msg=audit(1478754990.835:4149): avc:  denied  { read } for  pid=15626 comm="zabbix_server" name="snmptt.log" dev="xvda1" ino=75528756 scontext=system_u:system_r:zabbix_t:s0 tcontext=system_u:object_r:snmpd_log_t:s0 tclass=file"

shell> echo "type=SYSCALL msg=audit(1478754989.834:4148): arch=c000003e syscall=2 success=no exit=-13 a0=25de540 a1=0 a2=7ffe07dedda0 a3=0 items=0 ppid=15596 pid=15626 auid=4294967295 uid=995 gid=992 euid=995 suid=995 fsuid=995 egid=992 sgid=992 fsgid=992 tty=(none) ses=4294967295 comm="zabbix_server" exe="/usr/sbin/zabbix_server_mysql" subj=system_u:system_r:zabbix_t:s0 key=(null)
type=AVC msg=audit(1478754990.835:4149): avc:  denied  { read } for  pid=15626 comm="zabbix_server" name="snmptt.log" dev="xvda1" ino=75528756 scontext=system_u:system_r:zabbix_t:s0 tcontext=system_u:object_r:snmpd_log_t:s0 tclass=file" | audit2allow -M zabbixsemanage
******************** IMPORTANT ***********************
To make this policy package active, execute:

semodule -i zabbixsemanage.pp

shell> ll
total 8
-rw-r--r--. 1 root root 942 Nov 10 05:38 zabbixsemanage.pp
-rw-r--r--. 1 root root 172 Nov 10 05:38 zabbixsemanage.te

shell> semodule -i zabbixsemanage.pp
```

* 网络上的安全问题
  - 在云上，注意安全组没有阻止了对应SNMP的端口的访问。
    + port: 161, protocol: UDP
    + zabbix agent: 10050, protocol: TCP

* 绑定SNMP Trap模板的limitation
  - 本想通过建立SNMP Trap的template，这样就能方便地通过zabbix自发现或者探针自动注册的方式，通过`action`把`SNMP Trap template`绑在被监测机上。但是zabbix目前没有自动发现SNMP interface的功能，所以会导致`action`里面所有的绑定tempalte得操作失败。所以，对于`SNMP Trap`的功能，需要在`zabbix server`上为每一台新加入的被检测机配置`SNMP Trap Interface`并绑定我们自定义做好的`SNMP Trap Template`
  - https://support.zabbix.com/browse/ZBXNEXT-1212

# Reference #
* [SNMP traps - zabbix official](https://www.zabbix.com/documentation/3.2/manual/config/items/itemtypes/snmptrap)
* [浅谈 Linux 系统中的 SNMP Trap](https://www.ibm.com/developerworks/cn/linux/l-cn-snmp/)
