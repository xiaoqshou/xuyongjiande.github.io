---
layout: post
title: First use of Python
description: First use of Python with input and output functions, also learned about while, if and logic operator and, or, not.
classification: Python
---

I heard the words: "Late is better than never".

Finally I start the learning of Python, what I should have done at least one year ago.  

This is my first try of python. It is very small, but contains the basic struct.

```
#!/usr/bin/env python
# -*- coding: UTF-8 -*-

yourInput = ""
inputTime = 0
while (yourInput != '' or inputTime == 0):
    yourInput = raw_input()
    inputTime += 1
    if yourInput:
        print "for the", inputTime,  "time, your input is: ", yourInput, "."
```

I got these notes in this procedure.

* num++ is not allowed in Python, it should be num += 1
* variables you want output should divided by ',', which will be changed to ' ' automatically
* there is no '&&' '||' in Python, Python uses English words "and", "not", "or" to represent logical operation
* Another difference with shell is: variables in python must be defined before use.

I realized this looks ugly:

```
print "for the", inputTime,  "time, your input is: ", yourInput, "."
```

Then I got: Python supports format-strings like C.  
So, I can use:

```
print 'For the %d time, your input is: %s.' %(inputTime, yourInput)
#Notes that the '%' before '(inputTime, yourInput)' is needed.
```

* Unicode and UTF-8

	Unicode is supported by python. Use it like this `print u'你猜'`, I also found `print '你猜'` works well in python 2.7, but there is still differences between those two. `len('你猜')` returns 6, but `len(u'你猜')` returns 2.

	Use one of them by your need.
