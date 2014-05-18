---
layout: post
title: Build-in Structure in Python
description: 
classification: Python
---

####list

example:

```
>>> names = ["Mike", "Jake", "Michelle"]
>>> print names[0]
Mike
>>> len(names)
3
```

above is easy to understand, but python also support this:

```
#start from the end of list, -1 means the last one
>>> print names[-1]
Michelle
>>> print names[-3]
Mike
>>> print names[-4]
IndexError:list index out of range
```

What makes Python more easier is the list can be changed in an elegant way.

```
>>> names.append("tom")
>>> names
['Mike', 'Jake', 'Michelle', 'tom']
>>> names.pop()
'tom'
>>> names
['Mike', 'Jake', 'Michelle']
>>> names.insert(0, "Tom")
>>> names
['Tom', 'Mike', 'Jake', 'Michelle']
>>> names.pop(0)
'Tom'
>>> names
['Mike', 'Jake', 'Michelle']
```

members in list can be any type, So multidimensional list is support. That means listB is a member of listA, which looks like:

```
>>> A1=['a1', 1]#Yeah, A1[0] is string, but A1[1] is int. This is allowed.
>>> A2=['a2', 2]
>>> B=[A1, A2]
>>> B
[['a1', 1], ['a2', 2]]
```

####tuple

example:

```
>>> tuple_example=()
>>> print tuple_example
()
>>> tuple_example=(1,2)
>>> print tuple_example
(1, 2)
>>> tuple_example=(1)
>>> print tuple_example
1
>>> tuple_example=(1,)
>>> print tuple_example
(1,)
>>> tuple_example[0]
1
>>> tuple_example[0]=2
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```
* keypoints:  
	1. tuple can not change, you get more safety by using tuple than list if there is no need to change it.
	2. if your tuple has one element, it must be defined like`tuple_example=(1,)` not `tuple_example=(1)`, the second one is the same as `tuple_example=1`, which defines an integer not a tuple, because '()' is a mathematical operator.
	3. if one element in a tuple is a list, it must be known that the elements in this list can be changed.

####dict

```
>>> dict_example={"wang": 60, "zhang": 70, "li": 90}
>>> dict_example['wang']
60
>>> dict_example['xu']=65
>>> dict_example['xu']
65
>>> len(dict_example)
4
>>> dict_example['xu']=100
>>> dict_example['xu']
100
>>> #xu-65 become xu-100, 
>>> dict_example['notexist']
KeyError: 'notexist'
>>> name='xu'
>>> if name in dict_example:
...     print dict_example[name]
... 
100
```
* to get value from a dict, dict_example['xx'] may get error, actually there is a better way:

```
>>> dict_example.get('xu')
100
>>> dict_example.get('no') # if not exist, return None
>>> dict_example.get('no', 0) # if not exist, return the second parameter
0
>>> if dict_example.get('no') == None:
...     print "Not exist!"
... 
Not exist!
```

####set

Define a set is not like list, tuple or dict, list uses '[,]', tuple '(,)', and dict uses '{:,}'. A set should be `set_example=set()` or `set_example = set(listA)`.

```
>>> listA=['A1', 'A2']
>>> set_example=set(listA)
>>> set_example
set(['A1', 'A2'])

# element of set should be values or structures hashable.
# so list(which is unhashable) can not be a element of a set.
# actually, set is like map, but it just has the 'key' not 'key-value'
>>> listlistB=[listA, listA]
>>> set_example=set(listlistB) 
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'

>>> set_example=set()
>>> set_example
set([])
>>> set_example.add(1)
>>> set_example.add("1")
>>> set_example
set(['1', 1])
>>> set_example.remove(1)
>>> set_example
set(['1'])
>>> set1=set()
>>> set2=set()
>>> set1.add(1)
>>> set1.add(2)
>>> set2.add(2)
>>> set2.add(3)

# set can use & or |
>>> set1 & set2
set([2])
>>> set1 | set2
set([1, 2, 3])

# list can not use & or |
>>> listA=[1,2]
>>> listB=[2,3]
>>> listA & listB
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for &: 'list' and 'list'
```

#### About Hashable

```
>>> tupleA=(1,2)
>>> tupleB=(3,4)
>>> setA=set()
>>> setA.add(tupleA)
>>> setA.add(tupleB)

# tuple is hashable, but if tuple contains a unhashable structure(say a list)..., it becomes unhashable.
>>> tupleC=(5,[6])
>>> setA.add(tupleC)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```
