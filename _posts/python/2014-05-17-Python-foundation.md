---
layout: post
title: First use of Python
description: First use of Python with input and output functions, also learned about while, if and logic operator and, or, not.
classification: Python
---

I heard the words: "Late is better than never".

Finally I start the learning of Python, what I should do at least one year ago.  

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
        print "你第", inputTime,  "次输入的是: ", yourInput
```

I got these notes in this procedure.

* num++ is not allowed in Python, which should be num += 1
* variables need to be output should divided by ',', which will be changed to ' ' automatically
* there is no '&&' '||' in Python, Python uses English words "and" "not" and "or" to represent logical opration
* A difference with shell is: variables in python must be defined before use.
