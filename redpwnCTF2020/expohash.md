**Expohash (misc/35 solves)**

Solve script:

This challenge asked you to solve for 100000 values in a password, given a list of the computed XORs of subarrays. I used z3 to solve this challenge by asking z3 to solve for all `L-1^R=XORed_value` in the given ranges to the z3 solver, then computing the XOR of each item in the model with the one before it to get the actual value in the password.

Credits to PMP for helping me setup the z3 solver.

```python
from pwn import *
from z3 import *
r = remote('2020.redpwnc.tf', 31458) #connect 
s = Solver()
arr=[BitVec("x" + str(i).rjust(7, "0"), 32) for i in range(100000)] #setup array for z3
for i in range(100000): #parse input
    liss = r.recvline().split(b" ")
    if int(liss[0])>1:
        s.add(arr[int(liss[0])-2] ^ arr[int(liss[1])-1] == int(liss[2]))
    else:
        s.add(arr[int(liss[1])-1] == int(liss[2]))
print(s.check()) #check for SAT
m = (s.model())
a= (sorted ([(d, m[d]) for d in m], key = lambda x: str(x[0]))) #sort
included=[]
fin = []
for z in a: 
    included.append(str(z[0])) #adding to the final array
    fin.append([str(z[0]),z[1].as_long()]) 
for i in range(100000):
    if ("x"+(str(i).rjust(7, "0"), 32)[0]) not in included: #check to see what values aren't included in the final z3 model
        fin.append([str("x" + str(i).rjust(7, "0")),0]) #add it if it's not in
fin.sort(key=lambda x: x[0]) #sort again
r.sendline(str(fin[0][1])) #send edge case, because the initial value doesn't have to be XORed

for asdf in range(1,100000):
    r.sendline(str(int(fin[asdf-1][1]^fin[asdf][1]))) #send everything else
r.interactive()
```

Flag: flag{1_g0t_th3_c0mb_thx}
