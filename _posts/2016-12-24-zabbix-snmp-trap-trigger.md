---
layout: post
title: zabbix snmp trap trigger
description: "SNMP trap triggers in zabbix"
tags: [zabbix, devops]
image:
  background: triangular.png
---

# Zabbix SNMP Trap Trigger

* Zabbix 提供了trigger（触发器）的功能，用于检测某些事件的发生，从而触发用户通知。
* 对于SNMP Trap，可以充分利用trigger的触发机制，定制需要的用户警告与通知。

# Zabbix SNMP Trigger Event

* Zabbix v3.2之后提供了一个很便捷的功能，对同一trigger所产生的trigger进行tag（标签），产生不同的事件。
  - 简单说，如果trap的内容里面如果包含以下
    + Source IP: 描述当前trap所产生的节点
    + Module: 描述当前trap所产生的应用/模块
    + Problem: 描述了trap产生的原因
    + Severity: 描述严重性
    + ErrorCode：描述错误码
    + Timestamp: 描述当前trap发起的时间
  - 在Zabbix的trigger配置里面，用 *Problem expression* 正则表达式，根据trap的内容去匹配，来产生trigger。并且能提供了用正则表达式去把上述特征从内容里面提取出来，作为trigger事件的tag。
  - zabbix还提供了清理对应事件的机制。 通过 *Recovery expression* 正则表达式匹配出用于清理问题的trap，自定义事件对应的唯一ID (*Tag for matching*)，用于问题事件和清理事件的关联，从而清理事件能把对应的问题事件进行修正。
  - 问题清理事件同样有通知功能。

![grafana-event-panel](https://raw.githubusercontent.com/KasperDeng/kasperdeng.github.io/master/images/zabbix/zabbix_trigger_config.jpg)

## 优点
* 尤其对于用SNMP trap去上报同一个系统的不同的状态，不同的事件是十分便利的。可以通过统一的机制，进行标签分类，划分出不同的事件。

## 缺点
* 虽然zabbix提供了tag对事件进行分类，但是在页面显示的展现方面还是不完善。
  - 事件只有自定义的标签，仍然沿用该trigger的严重程度(Severity)，如果相关的事件都由一个trigger所产生的，所有相关的事件都处于同一个严重程度。具体看下图。

# Zabbix Event Dashboard
* Problem事件都在监控下的Problem页面呈现。未被处理的事件处于红色的Problem状态。已解决的事件处于绿色的**Resovled**状态，后续会消失掉。
* Problem页面的缺点
  - 如上说，同一trigger下的事件无法根据内容设置严重程度，都是显示该trigger的严重程度。
  - 事件的标签如果比较多的话，不能在页面全部显示，龟缩在缩略列表，要手动点开去看，比较麻烦。
  - 事件的标签默认是按字母序排列，想让比较重要的标签（比如问题描述）优先显示，就要好好考量下tag的名称。

![zabbix-problem-panel](https://raw.githubusercontent.com/KasperDeng/kasperdeng.github.io/master/images/zabbix/zabbix_problem_panel.jpg)

## Grafana-Zabbix
* 阿历山大同学的 [grafana-zabbix](https://github.com/alexanderzobnin/grafana-zabbix) 插件有一个trigger面板的功能，但跟zabbix的problem页面遇到的问题是一样的，只是样式比较好看而已。
* 基于阿历山大同学的插件，Kasper同学的 [grafana-zabbix](https://github.com/kasperdeng/grafana-zabbix) 里面新增了一个事件面板，用表格的方式呈现事件中的标签内容，默认的标签key要有：
  - Source Host
  - Severity
  - Problem
  - module
  - ErrorCode
* `Severity` tag是用于解决zabbix页面中事件严重程度的问题，所以trap中有最好带上`Severity`的内容。
* 事件表格能够按事件的严重性和age进行排序。
* 事件表格能够按照事件的状态呈现不同的颜色。已解决的事件会显示它被哪个事件(EventId)处理的，并显示绿色的样式，表示*Resolved*。
* 插件还增强了Annotation(show events on graph)的功能，能把事件中所有的tag在graph里面显示出来。

![grafana-event-panel](https://raw.githubusercontent.com/KasperDeng/kasperdeng.github.io/master/images/zabbix/grafana_event_panel.jpg)

![grafana-annotation](https://raw.githubusercontent.com/KasperDeng/kasperdeng.github.io/master/images/zabbix/grafana_annotation.jpg)

