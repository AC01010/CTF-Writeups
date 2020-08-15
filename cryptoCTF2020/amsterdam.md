**Amsterdam**

**55 points, 96 solves**

This challenge was split into two parts: the first generated a number that represented the flag as a sum of combinations. The second part used the number generated from part one and encoded it into a ternary string, whos 1s and 2s each represented either the operation 2(x-1) or 2(x+1). In addition, each digit also multiplied the number by two.

```python
import numpy as np
import scipy.misc as sc
enc = 5550332817876280162274999855997378479609235817133438293571677699650886802393479724923012712512679874728166741238894341948016359931375508700911359897203801700186950730629587624939700035031277025534500760060328480444149259318830785583493
def pt1(num):
    copy = (np.base_repr(num,base=3))
    r = 1
    for i in range(1,len(copy)):
        a = int(copy[i])
        r*=2
        if a==1:
            r+=1
        elif a==2:
            r-=1
    return r
def pt2(num):
    n,k,r = -1,0,0
    for i,c in enumerate(bin(num)[:2:-1]):
        n+=1
        if c=='1':
            k+=1
            r+=sc.comb(n,k, exact=1)
    return r
print(bytes.fromhex(hex(pt2(pt1(enc)))[2:]).decode('utf-8'))
```

Flag: 
**CCTF{With_Re3p3ct_for_Sch4lkwijk_dec3nt_Encoding!}**
