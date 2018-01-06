---
layout: post
title: golang-notes
description: "golang-notes"
tags: [golang]
image:
  background: triangular.png
---

# 序 #
终于有点时间重新回到正常的学习节奏。心里念念不忘GO语言，于是重温一下GO语言的语法，顺便记录下笔记。希望成为一个能实战的GO程序员。


# 哲学，思想 #
* "21世纪的C语言"
* 简洁编程哲学的宣言，从长远来看，简洁依然是好软件的关键因素
* Go从C语言继承了相似的表达式语法、控制流结构、基础数据类型、调用参数传值、指针等很多思想

# 变量 #
* 定义
  - 采用了Javascript相同的关键字'var'和类型推断。但GO语言是强类型语言，所以可显式定义类型，变量类型的声明写在变量名称之后。
* 初始化
  - 零值初始化，在语言层面上保证值的初始化
    + 数值类型变量对应的零值是0
    + 布尔类型变量对应的零值是false
    + 字符串类型对应的零值是空字符串
    + 接口或引用类型（包括slice、指针、map、chan和函数）变量对应的零值是nil
  - 初始化表达式可以是字面量或任意的表达式(e.g. new(T))
    + 包级变量会在main执行前完成初始化
  - 支持多变量初始化赋值，混合类型，平行赋值
* 变量的词法作用域/生命周期跟一般编程语言一样，就近原则(Principle of Proximity)。
  - 定义在函数体内，局部变量。生命周期随着引用不可达而被GC回收而终结。
    + 因为Go语言的指针和闭包，局部变量的生命周期与JS类似，并不一定随着函数退出而消亡。
    + 同时闭包也影响变量是在堆还是在栈上分配内存空间。
  - 定义在函数体外，包级变量。生命周期和整个程序的运行周期是一致的。
  - 声明方式的不同
    + 函数体内: 可用简短变量声明`:=`(声明并赋初值)，省略`var`，并由编译器自行类型推断
    + 函数体外：必须以`var`开头

# 常量 #
* 关键字`const`定义和类型推断。

# 类型 #
* Go是强类型语言
  - 类型转换需要**显式**转换: T(v)将值v转换为类型T

* 基本数据类型

~~~golang
bool
string
int int8 int16 int32 int64
uint uint8 uint16 uint32 uint64 uintptr
byte //uint8的别名
rune //uint32的别名，代表一个unicode码
float32 float64
complex64 complex128
~~~

* 其他变量不能强制当作布尔类型使用
* int, uint, uintptr: 32/64 bit int, flatform dependent
* GO把复数作为基本类型，对于高级数学运算，深度学习有帮助。GONUM或许将会成为numpy的强大对手。

* 复合数据类型

# 指针 #
* 和C++一样，&是取地址符
- `p`代表的是变量 i 的内存地址，类型是`*int`
- `*p`对应p指针指向的变量的值

~~~golang
i := 1
p := &i
*p = 2
~~~

* 有指针但没有指针运算

# 表达式 #
* "++"、"--" 是语句而非表达式

~~~golang
b := n++ // syntax error
if n++ == 1 {} // syntax error
++n // syntax error
~~~

* 不支持三元操作符 `a > b ? a : b`
* range
  - range 会复制对象
  - range 忽略第2个返回值，支持 string/array/slice/map

# 函数 #


# 控制流 #
* for
  - Go语言只有关键字`for`用于循环

~~~golang

for i:=1; i < 10; i++ {
   //doSomething()
}

// while
for i < 10 {
   //doSomething()
   i += 1

}

// death-loop

for {

}
~~~

* if
  - if 可以在条件之前执行一个简单的语句

* switch
  - 与C，Java不同，默认匹配的分支会自动终止，无须`break`，想贯穿的话，显式地在分支加fallthrough语句

# Debug #
* fmt.Printf
  - %T: print type. e.g. `fmt.Printf("i is of type %T\n", i)`

# Reference #
* [Go语言圣经（中文版）](https://docs.hacknode.org/gopl-zh/)
* Go语言圣经中文版-github
  - https://github.com/golang-china/gopl-zh
  - [源文件构建离线版本](https://github.com/gopl-zh/gopl-zh.github.com)
* [Go语言圣经英文版-gopl](http://www.gopl.io/)