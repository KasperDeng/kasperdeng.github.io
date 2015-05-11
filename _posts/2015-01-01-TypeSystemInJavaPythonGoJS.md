---
layout: post
title:  type system in Java/Python/Go/Javascript
description: "type system in Java/Python/Go/Javascript"
tags: [programming]
image:
  background: triangular.png
---

# Type determination #
* java interface query:
   - interfaceDemo `instanceof` InterfaceDemo
   - subClassInstance `instanceof` ParentClass
* python
   - isinstance
* golang interface query:  
   - value, ok := element.(T)    `element is interface var, T is base type`
   - switch value := element.(type)

# Cast #
* golang not support implicit type cast
* tuncation
   - small type casts to large type is safe
   - large -> small causes tuncations, only keeps the data in low address. The situation depends on what saves in low address (big-endian/little-endian)

~~~
/*** java ***/
double doubleDemo = 0.1d;
int intDemo = (int) doubleDemo ;

### python ###
>>> type(str(123))
<type 'str'>

>>> bin(17), int('0b10001', 2)
>>> oct(20), int('024', 8)
>>> hex(22), int('0x16', 16)

# suuport string ~ container(list, tuple, dict, set) conversion
>>> str([0, 1, 2]), eval("[0, 1, 2]")

/*** go ***/
var int32Demo int32 = 12345
var int64Demo int64 = int64(int32Demo)

switch value := element.(type) {
case string:
   ...
case []byte:
case int:
default:
}
~~~

# Primitive #
* Java:
   - null
   - boolean, byte, char(2 bytes), short, int, long, float, double
   - fraction literal is double by default. e.g. 3.14 is double type
* Python: everything is object
   - None
   - bool
   - int, long, float, complex
   - sequence: str, unicode, list, tuple
   - dict
   - set, frozenset

~~~
>>> map(bool, [None, 0, "", u"", list(), tuple(), dict(), set(), frozenset()])
[False, False, False, False, False, False, False, False, False]
>>> int(True), int(False)
(1, 0)
~~~

* Go:
   - nil
   - bool
   - int8, uint8, int16, int32, int64, int, uint, uintptr
   - float32, float64
   - complex64, complex128
   - string
   - byte(utf-8), rune(unicode), **no char**
   - error
   - compound type: array, slice, map, chan, struct, pointer, interface

~~~
demoInt := 2
if demoInt {
        fmt.Println("int value can be used by if")
}
error output: non-bool demoInt (type int) used as if condition
~~~

* Javascript: 
   - primitive type
      - undefined: undefined
      - null: null - can be treated as a specifal value of Object
      - boolean: true, false
      - number: double (64bits, 8 bytes), NaN, Infinity
      - string: UTF16
   - object type
      - Object
         + wrapper: Number, Boolean, String

~~~
var n = 1, b = true, s = "test"
var Num = new Number(n);
var Boo = new Boolean(b);
var Str = new String(s);
~~~

   - check by typeof()
   - Object type (wrapper for primitive) has methods, except null and undefined