---
layout: post
title: generator in python
description:
classification: Python
---

We can get a new list by using List-Comprehensions in python, and it is very useful. But, if we only need to access the front of several elements of the new list, it is a waster that we generate the whole list. There is a better way to do so by using "generator".

#### What is a generator?

Generators allow you to declare a function behaves like an iterator, i.e. it can be used in a for loop.

Example:

```
>>> g = (x*x for x in range(1,4)) # uses (), different with [x*x for x in range(1,4)] which returns a whole new list
>>> g
<generator object <genexpr> at 0x7fa29b591cd0>
>>> g.next()
1
>>> g.next()
4
>>> g.next()
9
>>> g.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  StopIteration

```

Use generator in a for loop(more common than using g.next()).

```
>>> for i in g:
...     print i
... 
1
4
9
```

#### More complicated generators

We can also def a generator just looks like a function, by using 'yield'.

```
>>> def g(num):
...     for x in num:
...             yield x*x
... 
>>> list1=range(1,1001)
>>> g(list1).next()
1
>>> g(list1).next()
1
>>> g(list1).next()
1
>>> gg=g(list1)
>>> gg.next()
1
>>> gg.next()
4
>>> gg.next()
9

listnew=list(g(list1))
# now listnew is the whole list [1, 4, ..., 1000000]
```
