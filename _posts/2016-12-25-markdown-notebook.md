---
layout: post
title: markdown 笔记软件
description: "markdown notebook"
tags: [SDE]
image:
  background: triangular.png
---

# 需求
作为一名码农，开发，设计，测试啥的，都需要用笔记软件记录一下tips，心得，体会。在周边，会发现Evernote, 有道笔记，为知笔记基本上是最为主流的。我个人比较喜欢用为知笔记，无他，因为它支持markdown，当然有道现在也支持了。

但'岁末迎新'，为知笔记在新的一年更改收费策略，全面铺开对个人用户收费(现在不单是屙乐扣会到处开炮，连个笔记软件都想绑架了你)。好吧，作为穷码农，我只好自己折腾了，把之前的为知笔记全部export出来，准备找个类似的……

但万万没想到，还真不容易找到一个合适的……

## 具体需求如下 ##
* Mandatory
    - 既然笔记软件，一定要集成笔记文件管理器
    - 要有只读模式，而不是那种左右分屏的markdown编辑器
    - 支持markdown（最好高级点，能支持LATEX公式，时序图，甘特图等）
    - 支持VIM模式（vi狂魔）
    - 开源（方便我自己去捣鼓，有啥不顺能自己改，而且不会被再绑架）
    - 一定要有漂亮的样式，主题
* Optional
    - 能打tag
    - 能免费同步当然最好，但容易被绑架。我打算自己手工同步
    - 能直接分享
    - 全平台：Windows, Linux, Mac, IOS, android...... （这个太变态了。。。）

需求挺多……， 找了半天，最大的发现就是Mac上很多这类型的笔记软件，但Windows上去很少。 Mac却有很多，尤其有像[Marboo](http://marboo.io/)(码薄笔记，。。。这名字)这样简洁，界面清新，功能简单够用的。但只支持Mac啊，还要付费，我等穷鬼用不起。

先不说是否要全平台的，能找到类似的有这些，如果按类型分，基本就是一下几种类型。
* JS版，基于Electron (atom shell)
> Electron的缺点就是启动慢，但启动了后也不见得慢，笔记软件，也就启动一次而已啦。但优点突出，尤其在样式渲染，所以Electron desktop app一般都是全平台的，而且贴近Mac应用的酷炫。

  - [atom](https://atom.io): 程序员的编辑器，原生对markdown有很好的支持。作为markdown编辑器，是很不错的选择。同时稍微改造一下，可以成为笔记本软件，只要安装以下插件。
     + [markdown-preview-enhanced](https://github.com/shd101wyy/markdown-preview-enhanced): 对markdown preview提供更加强大的功能，比如，scroll sync, TOC, MathJax/KaTex, export PDF, HTML, JPG, Flowchart/Sequence diagram 以及preview css自定义 等等
     + [maximize-panes](https://atom.io/packages/maximize-panes): 最大化preivew panel，作为笔记的阅读模式。
  - [leanote](https://github.com/leanote/leanote): 中文叫*蚂蚁笔记*
     * 开源，国人开发，代码注释都是中文，亲切？
     * 全平台，包括手机端的IOS和安卓
     * 支持VIM和EMACS模式
     * [Leanote Desktop APP](https://github.com/leanote/desktop-app)是基于Electron开发的，开发，调试都很方便。
     * 用了NEDB做存储的，包含了title tags date categoris等meta。所以导入导出都需要做格式的转换，这些都由插件来完成。
        - 自身带了从Evernote和Leannote导入的插件。
        - [leanote导入导出MD工具](https://github.com/goodbest/Leanote4MD): 但只支持普通注册用户（通过请求服务端导入本地markdown文件），本地用户想通过插入NEDB的话，暂时没实现。
     * 支持本地账号。隐藏功能？从代码里面看到的，否则你必须要去官方注册个账号。
        - 要用本地账号，需要创建本地用户。把login.xml的页面里面创建本地用户的`local-form`的样式和普通用户登录框的`leanote-form`对调一下，就能显示出创建本地用户的页面了。
     * 主题不算太丰富，但够用。而且主题能定制。
     * 如果只用本地账号，可以将所有的NEDB数据库同步到网盘。下次导入覆盖就行了。当然，leanote官方也提供同步，有免费套餐和旗舰套餐，见：https://leanote.com/pricing
  - [haroopad](https://github.com/rhiokim/haroopad): [官网](http://pad.haroopress.com/)，韩国出品。可以没集成文件管理器，而且源码两年都没更新了。release版本v0.13.1跟github的tag都对不上号了。但作为markdown编辑器不错。本文就是在haroopad上写的。有空尝试改改它，加上文件管理器。
  - [another note](https://github.com/AnotherNote/anote):目前仅支持Mac，但开源，捣鼓下支持Windows不难。样式不错。

* Web版
  - [stackedit](https://github.com/benweet/stackedit)
  - [Python-Markdown-Editor](https://github.com/ncornette/Python-Markdown-Editor): 只能算是markdown编辑器了。

* python版，基于PyQt.4+/5+
  - [markeditor](http://markeditor.com/app/markeditor)：非开源，有据说不限时间的免费试用版。偶尔提醒你购买license，而且据说用得越久，提醒越频密（这招也狠）。几乎能满足我在Windows上所有需求，但是解释markdown却个别地方有问题。。。
  - [FarBox](https://www.farbox.com/service/app/desktop_editor): 可以与Farbox同步，几乎与上面的markeditor差不多。
  - [mikidown](https://github.com/ShadowKyogre/mikidown)

* C++版
  - [MdCharm](https://github.com/zhangshine/MdCharm): 啥都好，C++开发，启动速度也快，但样式真的不敢恭维
  - [QQWnNotes](https://github.com/pbek/QOwnNotes)：[官网](http://www.qownnotes.org/), 基于Qt 5.3+，强大得让人害怕。。。

* 另类
 - [Notes-up](https://github.com/Philip-Scott/Notes-up): 样式很符合我口味，但基于elementary OS，什么鬼……。原来[elementary OS](https://elementary.io/zh_CN/)是基于快速、开源的 Windows / macOS 替代方案。

* 纯粹的markdown编辑器
  - [markdownpad](http://www.markdownpad.com/)
  - [作业部落-CmdMarkdown](https://www.zybuluo.com/mdeditor)
  - [简书](http://www.jianshu.com/)
  - [小书匠](http://markdown.xiaoshujiang.com/): 功能强大，主题丰富，还能支持*竖排写作*，有Web版和离线版。赞一下。
  - [Typora](http://www.typora.io/):很特别的markdown编辑器， 没有左右分栏的preview. WYSIWYG - What You See Is What You Get
  - [moeditor](https://github.com/Moeditor/Moeditor):[官网](https://moeditor.org/)，开源，基于Electron。
  - [markdown-plus](https://github.com/tylingsoft/markdown-plus): [官网](http://tylingsoft.com/markdown-plus/)
