---
layout: post
title: Python List Comprehensions
description:
classification: Python
---

* The `range()` function

	`range()` is a buitin function in python. 
	
	`range(N)` in which N>0 returns a list [0..N-1]  
	`range(0)` returns []

	`range(M,N)` in which `M<N` returns a list [M..N-1]  
	`range(M,N)` in which `M>N` returns []  

* List Comprehensions

	* example: we want 1^2, 2^2...10^2, how can we get this list?

	```
	L=[]
	for i in range(1, 11):
		L.append(x * x)
	```
	above is ok, but is there an easier way?   
	Yes:

	```
	L=[x*x for x in range(1,11)]
	>>> L
	[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
	```

	As we can see, It is beautiful and easy to understand!
	
	Moreover:

	```
	>>> L=[x*x for x in range(1,11) if x%2==0]
	>>> L
	[4, 16, 36, 64, 100]

	```

	double(or more) cycle:


	```
	>>> L=[x*y for x in range(1,6) for y in range(6, 11)]
	>>> L
	[6, 7, 8, 9, 10, 12, 14, 16, 18, 20, 18, 21, 24, 27, 30, 24, 28, 32, 36, 40, 30, 35, 40, 45, 50]
	```
