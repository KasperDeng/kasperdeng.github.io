---
layout: post
title: 《go语言圣经》读书笔记
description: "golang-notes"
tags: [golang]
image:
  background: triangular.png
---

# 序 #
终于有点时间重新回到正常的学习节奏。心里念念不忘GO语言，于是重温一下GO语言的语法，顺便记录下笔记。希望成为一个能实战的GO程序员。


# 哲学，思想 #
* "21世纪的C语言"
* 简洁编程哲学的宣言，从长远来看，简洁依然是好软件的关键因素。
* Go从C语言继承了相似的表达式语法、控制流结构、基础数据类型、调用参数传值、指针等很多思想。

# 变量 #
* 定义
  - 采用了关键字`var`定义(Javascript类似)，编译器支持类型推断。但GO语言是强类型语言，所以可显式定义类型，变量类型的声明写在变量名称之后。
* 初始化
  - 零值初始化，在语言层面上保证值的初始化
    + 数值类型变量对应的零值是0
    + 布尔类型变量对应的零值是false
    + 字符串类型对应的零值是空字符串
    + 接口或引用类型(包括slice、指针、map、chan和函数)变量对应的零值是nil
  - 初始化表达式可以是字面量或任意的表达式(e.g. new(T))
    + 包级变量会在main执行前完成初始化
  - 支持多变量初始化赋值，混合类型，平行赋值

```golang
var s, n = "abc", 123 // 多变量赋值，混合类型的平行赋值
```

* 作用域
  - 类型
    + 全局作用域
    + 包级作用域: 包中声明的名字，d同一个包的任何源文件中访问
    + 源文件作用域: 对于某源文件**导入的包**，对同一包中的其他源文件不可见
    + 局部作用域: 例如函数内部
  - 变量的词法作用域(lexical scope)与一般编程语言一样，就近原则(Principle of Proximity)。
    + NOTE: 在"Go语言圣经" 2.7 "作用域" 一节中，着重强调了作用域与生命周期的概念
      - 声明语句的作用域对应的是一个源代码的文本区域；是一个编译时的属性。
      - 生命周期是指程序运行时变量存在的有效时间段，在此时间区域内它可以被程序的其他部分引用；是一个运行时的概念。
  - 定义在函数体内，局部变量。生命周期随着引用不可达而被GC回收而终结。
    + 因为Go语言的指针和闭包，局部变量的生命周期与JS类似，并不一定随着函数运行结束而消亡。
    + 同时闭包也影响变量是在堆还是在栈上分配内存空间。
  - 定义在函数体外，包级变量。生命周期和整个程序的运行周期是一致的。
  - 声明方式的不同
    + 函数体内: 可用简短变量声明`:=`(声明并赋初值)，省略`var`，并由编译器自行类型推断
    + 函数体外：必须以`var`开头

# 常量 #
* 关键字`const`定义常量，支持编译器类型推断。
* itoa: 定义常量组中从 0 开始按行计数的自自增枚举值。
* 常量值可以是len、cap、unsafe.Sizeof等编译期可确定结果的函数返回值[4]。

```
const (
a = "abc"
b = len(a)
c = unsafe.Sizeof(b)
)
```

# 类型 #

## Go是强类型语言 ##
* 类型转换需要**显式**转换: T(v)将值v转换为类型T (Java程序员对于Go的强制类型转换表达方式要适应一下了...)

## 基本数据类型 ##

| type          | length  | default  | note                      |  
| :----------:  | :-----: | :------: | :-----------------------: |  
| bool          | 1       | false    |                           |  
| byte          | 1       | 0        | uint8的别名               |  
| rune          | 4       | 0        | unicode码, uint32的别名   |  
| int, uint     | 4 or 8  | 0        | 32/64bit，依赖平台编译器  |  
| int8, uint8   | 1       | 0        | -128 ~ 127, 0 ~ 255       |  
| int16, uint16 | 2       | 0        | -32768 ~ 32767, 0 ~ 65535 |  
| int32, uint32 | 4       | 0        | -21亿 ~ 21亿, 0 ~ 42亿    |  
| int64, uint64 | 8       | 0        |                           |  
| float32       | 4       | 0.0      |                           |  
| float64       | 8       | 0.0      |                           |  
| complex64     | 8       |          |                           |  
| complex128    | 16      |          |                           |  
| uintptr       | 4 or 8  |          | 可存储指针的uint32/uint64 |  
| array         |         |          | 值类型                    |  
| struct        |         |          | 值类型                    |  
| string        |         | ""       | UTF-8 字符串              |  
| slice         |         | nil      | 引用类型                  |  
| map           |         | nil      | 引用类型                  |  
| channel       |         | nil      | 引用类型                  |  
| interface     |         | nil      | 接口                      |  
| function      |         | nil      | 函数                      |  
 
 
 * 其他变量不能强制当作布尔类型使用
 * GO把复数作为基本类型，对于高级数学运算，深度学习有帮助。GONU M或许将会成为numpy的强大对手。
  
## 复合数据类型 ##
* 数组的长度必须是常量表达式，因为数组的长度需要在编译阶段确定
* 数组依然很少用作函数参数；相反，我们一般使用slice来替代数组
* 数组和结构体是有固定内存大小的数据结构
* slice和map是变长的序列，动态的数据结构

### 数组 ###
* 由同构的元素组成

### 结构体 ###
* 由异构的元素组成
* 一个命名为S的结构体类型将不能再包含S类型的成员：因为一个聚合的值不能包含它自身。
* 一个命名为S类型的结构体可以包含`*S`指针类型的成员，以创建递归的数据结构。
* 匿名成员
  - 匿名成员特性只是对访问嵌套成员的点运算符提供了简短的语法糖。
  - 结构体字面值并不能用匿名成员的语法糖的简短表示法来赋值，会导致编译错误。
  - 匿名成员在结构体的可见性由匿名成员类型的可导出性控制。


### slice ###
* slice底层引用一个匿名的数组对象。
* slice是轻量级的数据结构，一种高效率的(指针)、可变长的(长度)、预留扩展空间(容量)的数组。
  - 指针: 指向第一个slice元素对应的底层数组元素的地址，slice的第一个元素并不一定就是底层数组的第一个元素。
  - 长度：slice中元素的数目，长度不能超过容量。len函数返回slice的长度。
  - 容量: 从slice的开始位置到底层数据的结尾位置。cap函数返回slice的容量。
* 切片操作
  - s := array[i:j]
  - s[m:n], 0 ≤ m ≤ n ≤ cap(s)
    + 切片操作(m-n)超出cap(s)的上限将导致一个panic异常
    + 超出len(s)则是意味着扩展了slice，长度变大。
* 比较
  - 零值的slice等于nil。一个nil值的slice并没有底层数组。一个nil值的slice的长度和容量都是0
  - 非nil值的slice的长度和容量也是0的
* 构建

```golang
make([]T, len) // len == cap
make([]T, len, cap) // same as make([]T, cap)[:len]
```

![slice](https://books.studygolang.com/gopl-zh/images/ch4-01.png)


### map ###
* Go语言没有`Set`数据类型，采用节省内存(value是bool或者空结构体)的map当作set: map[string]struct{}

* 值类型 vs. 指针类型
  - 结构体可以作为函数的参数和返回值
  - 效率：较大的结构体通常会用指针的方式传入和返回
  - 函数内部修改结构体成员的话，必须用指针传入；在Go语言中，所有的函数参数都是值拷贝传入的。

#### 指针 ####
* `&`是取变量地址符(`p`指向变量`i`的内存地址)，`*p`透过指针访问目标对象，支持指针类型`*T`，支持指针的指针`**T` (和C++一样)

```golang
i := 1
p := &i // p的类型是*int(指针类型*T)
*p = 2  // 对应指针p指向的变量的值
```

* 不支持指针运算，比如p++。不支持 "->" 运算符 (和C++不一样)
* 空指针值:nil，非C++的NULL，非java的null。
* 指针类型装换：可以通过unsafe.Pointer和任意类型指针间进行转换。
* 可以通过将unsafe.Pointer转换成uintptr，配合unsafe.Offsetof变相进行指针运算[4]。
* 返回局部变量指针是安全的,编译器会根据需要将其分配在GC Heap上。

#### 变量内存[4] ####
* new
  - new计算类型大小,为其分配零值内存,返回指针。
* make
  - make只支持引用类型的map, slice, chan。
  - make会被编译器翻译成具体的创建/构造函数,由其分配内存和初始化成员结构,返回对象而非指针。

# 表达式 #
* "++"、"--" 是语句而非表达式

```golang
b := n++ // syntax error
if n++ == 1 {} // syntax error
++n // syntax error
```

* 不支持三元操作符 `a > b ? a : b`
* range
  - range 会复制对象
  - range 忽略第2个返回值，支持 string/array/slice/map

# 函数 #
> 函数是一等公民(javascript名言)

## 函数类型(函数标识符) ##
* 相同的函数类型和函数标识符：两个函数形式参数列表和返回值列表中的变量类型一一对应。
* 形参和返回值的变量名不影响函数标识符。

## 形参 ##
* 形参名：该形参在函数体中没被引用时，形参名可省略。
* 形参类型：具有相同类型的形参，不必为每个形参都写出参数类型，可以用省略参数类型的形式声明。
* 没有默认参数值(python有默认参数值的特性)。
* 形参是实参的拷贝，实参通过值传递方式赋值。
* 实参如果是引用类型(slice, map, pointer, channel, function)，可致函数间接修改实参。
* 可变参数
  - 参数列表的**最后**一个参数类型之前加上省略符号"..."
  - 可变参数在函数体中看作为切片
  - 可以直接将切片作为实参传递给可变参数函数，但需要在实参后加上省略号"..."

```golang
func sum(vals...int) int {
   // ...
}

values := []int{1, 2, 3, 4}
fmt.Println(sum(values...)) // slice as real argument for Variadic Functions 
```

## 返回值 ##
* 若返回值有显式的变量名，函数的return语句可以省略操作数(bare return)。bare return可以减少代码的重复，但是某些场景使得代码难以被理解，不宜过度使用。
* 支持多返回值(类似python的多返回值特性)
  - 多返回值的设计，使得可预料、可控制的错误得到良好的处理，而不用像那些将错误看作是异常的语言那样子通过抛/捕异常的方式来处理。
* 调用者可以通过`_`(空标识符，blank identifier)忽略没用的返回值。

## 作用域 ##
* 函数的形参和有名返回值作为函数最外层的局部变量，被存储在相同的词法块中。

## 函数体 ##
* 标准库中没有函数体的函数声明，该函数可能是native方法，不是以Go实现的。
* Go中大部分函数的代码结构几乎相同
  - 首先是一系列的初始检查，防止错误发生，之后是函数的实际逻辑。

## 函数值(Function Value, Closures(闭包)) ##
* 函数拥有类型，可以被赋值给其他变量，传递给函数，从函数返回。对函数值的调用类似函数调用。
* 函数值不可比较，因为函数值除了引用了代码，还记录了状态(闭包)。它只能跟其零值(nil)比，用于作初始值检查。

### 闭包 ###
* 闭包：词法闭包(Lexical Closure)的简称，是指引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。当函数离开创建环境后，依然持有其上下状态。
* 闭包中引用的是自由变量的内存地址，而不是某一刻的值，这些自由变量以及其引用的对象并没有被释放。
* 更多闭包与作用域的知识，可参考javascipt的书[You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS)或其[gitbook](https://maximdenisov.gitbooks.io/you-don-t-know-js/content/)，概念相通。

## 匿名函数(Anonymous Function) ##
* 通过函数字面量(function literal)定义
* 匿名函数可以访问完整的词法作用域。

## 异常 ##
* Deferred Function
  - 包含该defer语句的函数执行完毕时，defer后的函数会在在释放堆栈信息之前被执行。
  - defer语句采用压栈方式，执行顺序与声明顺序相反。
* panic(err)
  - panic函数接受任何值做输入参数，作为某种错误信息，以panic value的形式输出到日志。
  - panic会引起程序的崩溃，一般用于严重错误。
* recover()
  - recover会使程序从panic中恢复，并返回panic value。

# 控制流 #
* for
  - Go语言只有关键字`for`用于循环

```golang
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
```

* if
  - if 可以在条件之前执行一个简单的语句

```golang
if err := WaitForServer(url); err != nil {
    log.Fatalf("Site is down: %v\n", err)
}
```

* switch
  - 与C，Java不同，默认匹配的分支会自动终止，无须`break`，想贯穿的话，显式地在分支加fallthrough语句

# 调试相关 #
* fmt.Printf

| format       | example                             | output                                                             | note                                         |  
| :----------: | :---------------------------------: | :----------------------------------------------------------------: | :------------------------------------------: |  
| %T           | fmt.Printf("i is of type %T\n", i)  |                                                                    | print type of variable/function              |  
| %#v          | fmt.Printf("%#v\n", w)              | Wheel{Circle:Circle{Point:Point{X:42, Y:8}, Radius:5}, Spokes:20}  | # - print every member with name in struct   |  
| %p           | fmt.Printf("%p\n", p)               | 0x2101ef018                                                        | print pointer address                        |

* Deferred Function
  - 调试复杂程序时，defer机制可用于记录何时进入和退出函数，以及记录输入参数以及返回值。

# 参考 #
1. [Go语言圣经(中文版)](https://books.studygolang.com/gopl-zh/)
   - https://docs.hacknode.org/gopl-zh/
2. [Go语言圣经英文版-gopl](http://gopl.io)
   - [英文版在线](https://www.safaribooksonline.com/library/view/the-go-programming/9780134190570/)
3. Go语言圣经中文版-github
   - https://github.com/golang-china/gopl-zh
   - [源文件构建离线版本](https://github.com/gopl-zh/gopl-zh.github.com)
4. [雨痕-Go学习笔记第四版](https://github.com/qyuhen/book)