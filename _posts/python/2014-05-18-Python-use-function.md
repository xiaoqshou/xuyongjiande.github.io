---
layout: post
title: Use function in Python
classification: Python
description: Learn about how to define and use a function, and to learn builtin functions in Python.
---

* How to define a function

Python uses 'def' and ':' to define a function. See below.

```
def func_example(x, y):
	if x > y:
		return True
	else:
		return False
```

If you want to define a function, but have not designed it well, you may use 'pass' to define a 'do nothing' function.

```
def func_notDone():
	pass # also works in other places
```

* More usecases

Python can return more than one result(a tuple in fact). Use this feature like below.


```
>>> a=1
>>> b=2
>>> def fun_add1(x, y):
...     return x+1, y+1
... 
>>> c,d = fun_add1(a,b)
>>> c,d
(2, 3)
>>> print c,d
2 3
```

* Builtin functions

	+ Common used:

		- len(...)
		- max(...)
		- min(...)
		- sum(...)
		- round(...)
		- pow(...)
		- abs(...)
		- cmp(...) return negative zero or positive.
		- any(...)
		- all(...)
		- execfile(...) exec another python script
		- filter(...) used like `filter(m_func, m_sequece)`, if sequence is a tuple or string, return the same type, else return a list.
		- hash(...) hash(object) -> integer

	+ change type:
		- chr(...) int to char, 0 <= m_int <256
		- hex(...) int to hexstring
		- oct(...)
		- bool(...) if parameter is int, return True for not zero. if other obj, return True for not empty.
		- int(...)
		- float(...)
