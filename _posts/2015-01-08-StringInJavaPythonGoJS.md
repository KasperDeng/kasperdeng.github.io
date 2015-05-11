---
layout: post
title:  string in Java/Python/Go/Javascript
description: "string in Java/Python/Go/Javascript"
tags: [programming]
image:
  background: triangular.png
---

| Item             | Java             | Python      | Go                  | Javascript |
|:-----------------|:-----------------|:------------|:--------------------|:-----------|
|type              | String           | str/unicode | string              | N/A        |
|package           | import String    | primitive   | primitive           | N/A        |
|charset encoding  | determined by os | ascii       | utf-8, unicode(rune)| UCS-2      |
|immutable         | Y                | Y           | Y                   | Y          |

* [ascii, unicode, utf-8](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386819196283586a37629844456ca7e5a7faa9b94ee8000)
* [Unicode & Javascript, history of UCS-2 w/ UTF16](http://www.ruanyifeng.com/blog/2014/12/unicode.html)

{% highlight python %}
#!/usr/bin/env python
# -*- coding: utf-8 -*-

## Ascii Tools
>>> ord('A')
65
>>> chr(65)
'A'
{% endhighlight %}

# Declaration & Initialization #

{% highlight java %}
// Java
String demoA = null;
String demoB = "";

# python
# Prefer using single quote for char
demoA = 'It\'s a demo A.'
# Prefer using double quote for string to align with other language
demoB = "It\'s a demo B."
chinese = "中文"   #utf-8
uchinese = u"中文" #unicode
                                             
>>> demoC = """ It's a
... demo
... C
... """
>>> demoC
" It's a \ndemo \nC\n"
>>> r"It\'s demo"
"It\\'s demo"
>>> type(demoA)
<type 'str'>
>>> del demoC

# Operations
>>> "a" * 3
'aaa'

// Go
var demoA string;
demoB := "It's a demoB"

// Javascript
var demoStr = "demo string"
{% endhighlight %}

# Operations #

{% highlight java %}
### encoding ###
// java
System.getProperties().getProperty("file.encoding")

# python
sys.getdefaultencoding()
sys.setdefaultencoding("utf-8")
encode(arg1) # encode to specific encoding arg1, e.g. "utf-8", "gb2312"
decode() # -> unicode

### others ###
# python
"a" + "b", "-".join(["a", "b", "c"])
"a-b-c".split("-"), "a\nb\nc".splitlines() or splitlines(True)
startwith(), endwith()
lower(), upper()
strip(), lstrip(), rstrip()
replace(), find

// go
str1 + str2 // string concatination
len(s)        // get length of string
str1[i]       // get char from string
for i, ch := range str {
...
}

// Javascript
str1 = '\u0041' // or double quota "\u0041"
str1 += str2;
str.length  // string length is attribute. In Java, length() is method
str.toUpperCase()
str1 === str2
{% endhighlight %}

# format #

{% highlight java %}
// java

# python
# support as format in C
# %[(key)][flags(-(left alignment)/+/#(i.e. #c, #X) etc)][width][.precision]typecode(d/f/s/x etc)
# %% uses for '%' escape
>>> "%(key)s=%(value)d" % dict(key = "a", value = 10)
'a=10'

# format for container objects: list, dict
{field!convertflag:formatspec: [[fill]align][sign][#][0][width][.precision][typecode]}

>>>"test {demo}".format(demo="Kasper")
'test Kasper'
   
// go
// %d, %f, %s, %x, %c
fmt.Printf("The length of \"%s\" is %d \n", str, len(str))

{% endhighlight %}


hex(id(x)) - return obj id - memory address

