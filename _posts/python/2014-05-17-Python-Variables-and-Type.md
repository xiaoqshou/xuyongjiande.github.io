---
layout: post
title: Python variables and types
description:
classification: Python
---

#### Types

* Integer:  
  1, -1, 0xff00 are supported

* Float:  
  3.1415, 1.2e5, 1.2e-5 are supported

* String:   
  strings in Python looks like "abc" or 'abc'. Sometimes you need this: "I love \\"U\\"", which makes "'" is one char of your string.  
  `print -r"anything"` is allowed, every char in "anything" will be reserved.  
  Another usage which looks beautiful and simple is below:  

  ```
  print '''line1
  line2'''
  ```
* Boolean:  
  It must be True or False not true or false.  

  ```
  >>> True and False#this is python commond
  False#this is output
  ```

* None:  
  None is not 0, or other values. It means the value is empty or not exist.

#### Variables

Variable does not contain a defined type, not like C/C++ or Java.  
`a=1` defines a an integer and a==1 returns True, `a='1'` changes a to a string, and then a==1 returns False.
