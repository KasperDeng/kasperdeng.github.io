---
layout: post
title:  map in Java/Python/Go/Javascript
description: "map in Java/Python/Go/Javascript"
tags: [programming]
image:
  background: triangular.png
---

|Map      | Java                   | Python    | Go        |
|:--------|:-----------------------|:----------|:----------|
|type     | Map<...>, HashMap, etc | dict      | Map       |
|package  | import Map             | primitive | primitive |
|mutable  | Y                      | Y         | Y         |

# Notes #
* java
 
* python
   - OrderedDict
 
# Operations #
 
{% highlight python %}
 
### python ###
# create
demodict = {} # empty dict
demodict = {'a':'1', 'b':'2'}
demodict = dict(a=1, b=2)
demood = OrderedDict()

# retrieve
demodict.get(key)  # absent returns None
demodict.get(key, default)
demodict.keys(), demodict.values(), demodict.items()
demodict.iterkeys(), demodict.itervalues(), demodict.iteritems()     

# update
demodict[key] = new_value
demodict.update(otherdict) # merge
demodict.setdefault(key, value) # return existing value of key or new value of new key

# delete
demodict.pop(key), demodict.popitem()
del demodict[key]

# others #
>>> "b" in demodict
True

demodict.viewitems() # return type dict_items for set operation &(intersaction), | (union), -(difference), ^ (symmetric difference) 

# Convert #
>>> list = [(1,2), (3,4)]
>>> list
[(1, 2), (3, 4)]
>>> dict((x,y) for x,y in list)
{1: 2, 3: 4}
{% endhighlight %}

{% highlight go %}
/*** go ***/
// create
var demoMap map[string] demoStruct
demoMap = make(map[string], demoStruct, <initCap>) # initCap is optional

// retrieve
value, ok := demoMap[key]

// update
demoMap[key] = value

// delete
delete(demoMap, key)

{% endhighlight %}
