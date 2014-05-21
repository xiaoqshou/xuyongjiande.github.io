---
layout: post
title: python slice of range
description:
classification: Python
---

* Get elements from tuple and list

	When I want to get elements from a tuple or a list, what should I do? Use recursion? That ok! But what else?

	Now, python give us a new choice!`tupleE[:10]` help us get tuple[0] ~ tuple[9] as a tuple.

	```
	>>> tupleE=(0,1,2,3,4,5)
	>>> a0,a1,a2=tupleE[:3] # [0:3] is the same --- Not [0:2] or [1:3]
	>>> print a0,a1,a2
	0 1 2
	>>> a0,a1,a2=tupleE[-3:] # --- Not [-3:0] or [-3:-1]
	>>> print a0,a1,a2
	3 4 5
	```

	[m:n] (m>=0 and n>=0) means: tuple[m]..tuple[n-1], tuple[m:m] returns a empty tuple.  
	[:n] equals [0:n]

	[-m:-n] (m>=0 and n>0) means: tuple[-m]..tuple[-(n+1)], tuple[-1] is the last one.  
	But [-m:] means tuple[-m]..tuple[-1] not equals tuple[-m:0] because tuple[-m:0] returns empty. Example: [-2:] means [-2] and [-1] , [-2:-1] means [-2], [-2:0] which should never shown up returns empty

	```
	>>> tupleE[-2:] # [-N:] is the only way to get last N elements.
	(4, 5)
	>>> tupleE[-2:-1]
	(4,)
	>>> tupleE[-2:0]
	()
	```

	To make it clear, we should better use [:n] [-m:], if our target is not in the middle.

	And it also works with a list, listE[m:n] returns a list.

* another feature

	Take one every N elements, this is excellent.

	```
	>>> tupleE[::1]
	(0, 1, 2, 3, 4, 5)
	>>> tupleE[::2]
	(0, 2, 4)
	>>> tupleE[::3]
	(0, 3)
	```
