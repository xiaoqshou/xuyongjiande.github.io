---
layout: post
title: decorator in python
description:
classification: Python
---

1. Function in python can be seen as an object, it can be a return value or a parameter of another function. And a decorator is a function whose parameter is a function and returns a function. Suppose that we want to print the name of a function when it being called, we can do as below:

```
>>> def funcA():
...     print "A is called."
... 
>>> def callFuncA():
...     print "calling A..."
...     funcA()
... 
>>> callFuncA()
calling A...
A is called.
```

* And we can also do like this:

```
>>> def funcA(parameter):
...     print "parameter is : %s" % parameter
... 
>>> def decorator(func):
...     def newFunc(*args, **kw):
...             print 'calling %s().' % func.__name__
...             return func(*args, **kw)
...     return newFunc
... 
>>> funcA = decorator(funcA)
>>> funcA("test")
calling funcA().
parameter is : test
```

* Make it easier.

```
>>> def decorator(func):
...     def newFunc(*args, **kw):
...             print 'calling %s().' % func.__name__
...             return func(*args, **kw)
...     return newFunc
... 
>>> @decorator
... def funcA(parameter):
...     print 'parameter is : %s' % parameter
... 
>>> funcA("test")
calling funcA().
parameter is : test
```

Now, we have got the usage of decorator.
