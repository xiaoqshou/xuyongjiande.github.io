---
layout: post
Title: Divide Two Integers
classification: Algorithm Python
---

Divide two integers without using multiplication, division and mod operator.

```
class Solution:
    # @return an integer
    def divide(self, x, y):
        flag = False;
        if((x>0 and y<0) or (x<0 and y>0)):
            flag = True;
        x = abs(x)
        y = abs(y)
        xx = getL(x)
        yy = getL(y)
        res = 0;
        for i in range(0, xx-yy+1):
            if (x - (y<<(xx-yy-i)) >= 0):
                res |= 1<<(xx-yy-i);
                x -= y<<(xx-yy-i);

        if flag:
            return 0-res;
        return res
def getL(x):
    i = 1
    while (i<32):
        if (x>>i == 0):
           break;
        i += 1

    return i
```
