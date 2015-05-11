---
layout: post
title:  list in Java/Python/Go/Javascript
description: "list in Java/Python/Go/Javascript"
tags: [programming]
image:
  background: triangular.png
---

* Data structure stores a sequence of items in a list

|List         | Java                             | Python    | Go                  |
|:------------|:---------------------------------|:----------|:--------------------|
|type         | List<...>, Arraylist, LinkedList | list      | List                |
|package      | import List                      | primitive | container/list      |
|element type | hybrid: `List<Object>`           | hybrid    | hybrid: interface{} | 
|mutable      | Y                                | Y         | Y                   |

# Notes #
* java
   - LinkedList
   - ArrayList: efficient in random access, not synchronized (Vector is)
* python
   - Tuple: an special immutable list analog. Its immutable and readonly characteristic is good for concurrent programming
   - namedtuple

{% highlight python %}
>>> demoTuple = () # empty tuple
>>> demoTule = (1, 2)
>>> demoTuple = (1,) # Note, commar is needed

# namedtuple
>>> User = namedtuple("User", "name age")
>>> u = User("user1", 10)
{% endhighlight %}

* go
   - Element is a structure with next, previous pointer

# Operations #

{% highlight java %}
/*** Java ***/
// create
List<String> demoList = new ArrayList<String>();
demoList.add(element)  // new element is added in the end of list
iter.add(element)      // new element is added before position of iterator

// update
demoList.set(idx)
iter.set(newElement)

// retrieve
demoList.get(idx) // low efficiency in random access, can use array or ArrayList
Iterator iter = demoList.iterator();
listIterator iter = demoList.listIterator(); // listIterator extends Iterator<E> with previous()
iter.next()

// delete
iter.remove() // remove last visited item

// others
demoList.contains(item)
{% endhighlight %}

{% highlight python %}
### python ###
# create
demolist = [] # empty list, len(demolist) = 0
demolist = ["a", "b", "c"]
demolist = [x for x in range(3)]

# update
demolist[idx] = new_item
demolist.append(item), demolist.insert(idx, sth)
demolist.expand(other_list) # merge lists

# retrieve
demolist[i] # range: 0-len(demolist) ~ len(demolist) - 1

# Delete
demolist.pop(), demolist.pop(idx), demolist.remove(item)

# other
len(demolist)
demolist.count(item)
demolist.index(item, position)
demolist[1:-3] # slice

# conversion
demoarray = array.array("l", range(3))
                         |--- typecode (must be c, b, B, u, h, H, i, I, l, L, f or d)
>>> type(demoarray)
<type 'array.array'>
list = demoarray.tolist()
demoarray.fromlist(demolist)
{% endhighlight %}

{% highlight go %}
/*** Go ***/
// create
demoList = list.New()
demoList.PushFront(element), demoList.PushBack(element)

// retrieve
demoElement := demoList.Front(), demoList.Back()

// update
demoList.InsertAfter(element), demoList.InsertBefore(element)

// delete
demoList.Remove(element)

// Element
demoElement.Value
demoElement.Next(), demoElement.Prev()
{% endhighlight %}
