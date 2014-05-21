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

* Return more than one value.

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

* Default parameters

	Usage of default parameters is like C/C++.

	```
	def func(a, b=1):
		xxx
	```

	 But we should keep aware that the default value not being changeable, such like a list. Because this value is global and static(the meaning in C/C++), so, if our func changes the parameter, the default value changes. Here is the case:

	```
	>>> def func(a=[]):
	...     a.append("test");
	...     return a
	... 
	>>> print func()
	['test']
	>>> print func()
	['test', 'test']
	>>> print func()
	['test', 'test', 'test']
	```

* Variable parameters

	The num of parameters of a function is changeable. To archive this feature, python uses '*' to unpack a list, tuple or dict. Actually it can be seen as python automatically pack more than one parameters to a list, tuple or dict. Here is the usage:

	```
	>>> def func_orig(listE):
	...     for ele in listE:
	...             print ele
	... 
	>>> list1=[1,2,3]
	>>> func_orig(list1)
	1
	2
	3
	>>> func_orig(1,2,3)
	Traceback (most recent call last):
	File "<stdin>", line 1, in <module>
	TypeError: func_orig() takes exactly 1 argument (3 given)
	>>> def func_new(*listE):
	...     for ele in listE:
	...             print ele
	... 
	>>> func_new(1,2,3)
	1
	2
	3
	>>> func_new(list1) # here can seen as func_orig([list1])
	[1, 2, 3]
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
