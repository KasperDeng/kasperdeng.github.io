---
layout: post
title:  constant pool in Java/Python/Go/Javascript
description: "constant pool in Java/Python/Go/Javascript"
tags: [programming]
image:
  background: triangular.png
---

# == Java ==#

## What ##
- The constant pool contains the constants associated with the class or interface defined by the file. 
- **Constants** are stored in the constant pool.
   + literal strings
   + final variable values
   + fully qualified names of classes and interfaces
   + field names and descriptors
   + method names and descriptors
- **Symbolic references** to all types, fields, and methods used by a type are stored in the constant pool
   - it plays a central role in the dynamic linking of Java programs
- The constant pool is organized as a list of entries. A count of the number of entries in the list, constant_pool_count, precedes the actual list, constant_pool.

## Why ##
- For quick object creation. Just get one from pool if it exists, otherwise create one in the pool
- The runtime constant pool is a subset of the method area , different to heap memory for "new" object creation

## How ##
- Wrapper classes of primitive (Byte, Short, Integer, Long, Character, Boolean, no Float and Double) are implemented the constant pool.
- Byte, Short, Integer, Long, Character only create constants object in pool when value (-128~127]
- IntegerCache
   - default range (-128, 127]
   - high can be configured when VM init by property java.lang.Integer.IntegerCache.high property, but MAX is Integer.MAX_VALUE
- All literal strings and string-valued constant expressions are interned.
   - String.intern() // get from pool if exists or add this new String object to the pool

# Reference #
* [Java (JVM) Memory Types](http://javapapers.com/core-java/java-jvm-memory-types/)

# == python == #

## How ##
* int cache range [-5, 257)
* range() vs. xrange().  xrange uses an iterator instead of generating the entire list, so uses less memory
* String poolization
   - intern(string)

{% highlight python %}
>>> a=258;print hex(id(a))
0x6000838f8
>>> b=258;print hex(id(b))
0x60007bfd8
>>> a is b
False
>>> a=-6;print hex(id(a))
0x60007c008
>>> b=-6;print hex(id(b))
0x6000838f8
>>> a is b
False
>>> a=37;print hex(id(a))
0x600028c40
>>> b=37;print hex(id(a))
0x600028c40
>>> a is b
True

>>> a=1001;b=1001;print hex(id(a));print hex(id(b)); a is b;
0x6000838e0
0x6000838e0
True
{% endhighlight %}

# == golang == #
* TBD. The memory model of golang is something differents with other programming language.
