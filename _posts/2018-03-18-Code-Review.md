---
layout: post
title: Code Review
description: Code Review
tags: [Code Review, Code Inspection]
image:
  background: triangular.png
---

# 混乱 #
Code review，一个老生常谈的话题。最近看到产品开发过程中各种混乱的问题，代码质量的问题，觉得我们的Code Review的付出还是过于随意，简单，流程化。

# Code Review 是什么？ #

![TheSecretToCodeQuality](https://pbs.twimg.com/media/CgAbqE6W4AASmwe.jpg:large)

> wikipedia: Code review is systematic examination (sometimes referred to as peer review) of computer source code. It is intended to find mistakes overlooked in software development, improving the overall quality of software. 

* 从公司文化和制度上看，Code Review做得最好的，当属Google了。
* 参考了国内的各大互联网公司，真正把代码评审落地得很好的不多，同样面临着各种问题。既要符合团队的口味，又要契合公司的文化，又不能舍本逐末。
* code review是一种文化，不论对工程师而言还是对公司。

# Code Review 好处 #

![DefectDensityVs.Loc](https://www.ibm.com/developerworks/rational/library/11-proven-practices-for-peer-review/image001.gif)

* 减少代码中的问题，提高下游个团队(QA, Ops, DM)的幸福感。
* 营造平等的氛围， 激发团队的积极性。
* 熟悉项目和产品的代码，熟悉团队的代码规范。

# Code Review 该怎么做？#

## 代码评审 VS. 项目进度 ##
* 这两者不应该有矛盾，在大目标上应该是一致的。开发团队与项目管理需要达成共识。在操作上，时间进度，风控上要取得平衡，有时候也要特事特办。

* 个人比较赞同陈皓在[CODE REVIEW中的几个提示](https://coolshell.cn/articles/1302.html)的几个观点，也比较符合目前团队和公司。
  - Code reviews不应该承担发现代码错误的职责
    + 代码中的bug应该由UT，CT, FT，PT来覆盖
  - Code reviews不应该成为保证代码风格和编码标准的手段
    + 代码格式，风格应该通过线下的静态工具就可以分析，统一标准。
* Code Review的实现思路
* Code Review的meeting是很奢侈的，时间
* Code Review的代码量

## Code Review 流程 ##

![BestPracticesForThePerfectSecureCodeReview](https://www.checkmarx.com/wp-content/uploads/2016/02/Blog-Headers-6.png)

### 效率 ###

![largeReview](http://s2.quickmeme.com/img/0e/0ee676a657e4f108f1ff2c9a48d35b78823c2d8b9268922d6f5ace8fed9679fe.jpg)

* 轻量化的review
  - 按照一定的度量，分步骤分批量review，比如User Story或者task，避免代码推到重做的风险。
* 提升review效率
  1. 时间允许的情况下，最好先offline review一下，准备下问题和comments。
  2. 可按以下方式进行review
     a. Review设计思路
     b. Review设计模式
     c. Review框架代码
     d. Review最终代码

### 参与者 ###

![reviewer](http://mindbowser.com/wp-content/uploads/2016/07/Basic-Code-Reviewer-Code-Review-Checklist.png)

* key reviewers：他们的经验对于代码评审是至关重要的。有足够的key reviwer参与，才能更好提升评审的质量。
* peer reviewers and teammates：他们随时会接手未完成的工作，同时也培养key reviwer接班人。
* API Users：他们会关心最终给调用的API会是长什么样子的。
* Ops team：他们能提出比较多线上的经验与如何收集问题现场的建议。

### 会议 ###

![CodeReviewMeeting](https://image.slidesharecdn.com/hownottoruncodereviewspdf-141217143831-conversion-gate02/95/how-not-to-run-code-reviews-7-638.jpg?cb=1418827251)

* 时间不要太长，否则造成疲惫，评审质量下降。
* 气氛轻松，吃货就带点零食或者以下午茶的形式来搞。
* 重大争论点先放一下，线下再详细考量。

### 心态 ###

![TheFearInCodeReview](http://mindbowser.com/wp-content/uploads/2016/07/Code-Review-2.jpg)

> 无论是代码作者，还是评审者，都需要一种积极向上的正面的态度，作者需要能够虚心接受别人的建议，因为别人的建议是为了让你做得更好；评审者也需要以一种积极的正面的态度向作者提意见，因为那是和你在一个战壕里的战友。记住，你不是一段代码，你是一个人！

* 被review的人有一种被虐的感觉，需要放开胸怀接受建议，从错误和失败中学习。
* 能有人在Code Review和你争论，是一件挺不错的事情。

### 文化 ###
* 让code review成为一种习惯，一种文化。
* code review中总结到的best practices，可以归档到设计概要上做指导。

### 从Code Review中学习 ###
* 代码评审本身就是一个很好的knowledge sharing的过程，让团队中其他成员了解设计思路，实现方式。
* 相互学习，多思考如何使得设计与实现通用，优雅。修炼自己编码的能力，提升代码质量。

# Code Review 工具 #
* 工具对于代码评审来说不是重点，但能提升效率与问题的跟踪。
* 对于git来说，github的pull request是一种比较好的pre-commit代码评审方式，但比较依赖reviewer，需要一套比较完善的reviewer的机制，否则有阻碍项目的风险。
* 对应git做VCS来说，有以下这些合适的工具：
  - [Review Board](https://www.reviewboard.org/) - 免费，可以自己搭建。IDEA intellij和Eclipse都有集成插件。
  - [gerrit](https://www.gerritcodereview.com/) - 跟concluence等系统有机集成。
  - [crucible](https://www.atlassian.com/software/crucible) - one time payment.

# 参考 #
1. [awesome code review](https://github.com/joho/awesome-code-review)
2. [Code Review - An effective process to develop quality softwares](http://mindbowser.com/code-review/)
3. [What is Code Review](https://smartbear.com/learn/code-review/what-is-code-review/)
4. [Code Reviews in an Agile, Fast-Paced Environment](https://hackernoon.com/code-reviews-in-an-agile-fast-paced-environment-464d5e6ec860)
5. [让 Code Review成为一种习惯真的那么重要吗？](http://www.flickering.cn/uncategorized/2014/08/%E8%AE%A9-code-review%E6%88%90%E4%B8%BA%E4%B8%80%E7%A7%8D%E4%B9%A0%E6%83%AF/)
