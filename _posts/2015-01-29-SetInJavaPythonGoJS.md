---
layout: post
title:  set in Java/Python/Go/Javascript
description: "set in Java/Python/Go/Javascript"
tags: [programming]
image:
  background: triangular.png
---

|Set      | Java                   | Python    | Go        |
|:--------|:-----------------------|:----------|:----------|
|type     | Set<...>, HashSet, etc | set       | N/A       |
|package  | import Set             | primitive | N/A       |
|mutable  | Y                      | Y         | N/A       |

# Notes #
* Set is not sequence, cannot be got by index and slicing
* python
   - frozenset is immutable set
* go
   - No primitive set and related lib

# Operations #

~~~

### python ###

# create
demoset = set()
demoset = set(list) # input args is sequence
demoset.add(element)

# retrieve
demoset.pop()

# update
demoset.update(other_set) # merge

# delete
demoset.remove(element)
demoset.discard(element)

# others
&, |, -, ^
==, !=, >, >=, <, <=
~~~