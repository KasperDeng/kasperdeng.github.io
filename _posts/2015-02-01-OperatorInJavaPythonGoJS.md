---
layout: post
title:  operator in Java/Python/Go/Javascript
description: "operator in Java/Python/Go/Javascript"
tags: [programming]
image:
  background: triangular.png
---


|Operator                | Java                       | Go                        | Python       | Javascript       |
|:-----------------------|:---------------------------|:-------------------------:|:------------:|:-----------------|
|++, --                  | statement                  | not statement, expression | N/A          | supported        |
|logical                 | &,&#124;,&&,&#124;&#124;,! | &&,&#124;&#124;,!         | and, or, not | &&,&#124;&#124;,!|
|bitwise not             | ~x                         | ^x                        | ~x           | ~x               |
|bitwise and             | x & y                      | x & y                     | x & y        | x & y            |
|bitwise or              | x &#124; y                 | x &#124; y                | x &#124; y   | x &#124; y       |
|bitwise exclusive or    | x ^ y                      | x ^ y                     | x ^ y        | x ^ y            |
|bitwise clear (and not) | N/A                        | &^                        | N/A          | N/A              |
|?: (ternary op)         | supported                  | N/A                       | N/A          | supported        |

* Java: `&, |` also have the logical function. But it differents with `&&, ||` which have short circuiting function.
* javascript special operator
  - instanceof: Test object class
  - in: Test whether property exists, `prop in objectName`
    + in object: test property whether in object
    + ** in array**: test index (not element) whether in array
     ~~~
     var testInArray = [7, 8, 9];
     "0" in testInArray; // true
      1 in testInArray; // true
     "7" in testInArray; // false
     8 in testInArray; // false
     ~~~
  - delete: Remove a property
  - typeof: Determine type of operand
  - void: return undefined value
  - `>>>`: Shift right with zero extension
  - ===, !== : Test for strict equality/inequality, type and value should be tested.
  - !!x: get equality of boolean of x
* go
  - ++, --: only supports post increment/decrement

{% highlight java %}
/*** java ***/
Int demoInt2 = demoInt1++
++demoInt1
- x ==y, x != y // value equals or not
- x.equals(y) // same object
{% endhighlight %}

{% highlight python %}
### python ###
>>>a**b == pow(a, b)
>>>(a//b, a%b) == (a//b, a%b) == divmod(a, b)
>>> x == y, x !=y  # value equals or not, check __eq__
>>> x is y, x is not y  # same object or not, check pointer
x in y, x not in y
>>> abs(-5) == 5
True
>>> a = 2
>>> ~a
>>> from struct import pack
>>> pack('b', -1)
'\xff'
# build-in function for sequence, dict
min(), max(), sum(), cmp()
{% endhighlight %}

{% highlight go %}
/*** go ***/
demoInt2 := demoInt1++ // syntax error
++demoInt1 //syntax error
x := 2
x, ^x   // 0010 -> 1101 -> (1101 -1) -> 1100 -> 1011
{% endhighlight %}

