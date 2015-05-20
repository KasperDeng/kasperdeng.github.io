title: Go Overview
author:
   name: Kasper Deng
   url: http://kasperdeng.github.io
theme: sudodoki/reveal-cleaver-theme   
output: go-overview.html
encoding: utf-8

--

# Hello, Go!  ![golang-logo](../../images/golang-logo.png) 

## Go is expressive, concise, clean, and efficient.

--

### Why go?
* Performance and dev efficiency
  - "best of both worlds" model, combining the efficiency of a low-level systems language
  - Make programmers more productive
* Programming Philosophy: simple !!!
* Software engineering principle
  - godoc
  - gofmt
  - error handling: defer, panic, error type, multi return value

--

### History
* 三位武学宗师: Hate C++
* [Ken Thompson](http://en.wikiquote.org/wiki/Ken_Thompson): design C, creation of Unix & Plan 9
   - Go理所当然延用了C扎实的静态编译系统
* [Rob Pike](http://en.wikiquote.org/wiki/Rob_Pike): creation of Plan 9 from [Bell Lab](http://en.wikipedia.org/wiki/Bell_Labs)
   - Smalltalk和Java大师，Go就顺理成章地具备了轻盈灵活的动态身段
* [Robert Griesemer](http://en.wikipedia.org/wiki/Robert_Griesemer): Java HotSpot VM, Google's V8 JavaScript engine
   - 贡献的是Go语言存在的意义 - Go的灵魂 - 并发

--

### Praise 
* C语言是互联网时代的汇编语言, Go语言是互联网时代的C语言
* 大道至简，显式表达
  - 废弃大量的OOP特性: 如继承、构造/析构函数、虚函数、函数重载、默认参数等
  - 简化的符号访问权限控制
  - 将隐藏的this指针改为显式定义的receiver对象
  - strictly type checking
* It's a fast, statically typed, compiled language that feels like a dynamically typed, interpreted language.

--

### Advantage
* modern language: gc, runtime reflection
* Easy Deployment
* Good performance of concurrency (goroutine)
* Supports concurrency at the language level
* Fast compilation speed
* Be fast as C and intuitive and simple as python

--

### Disadvantage
* immature gc: stop-the-world
* No generics

