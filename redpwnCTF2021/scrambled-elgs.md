**scrambled-elgs**

*143 points, 70 solves*

The challenge is based off the cryptosystem [here](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.135.5778&rep=rep1&type=pdf).

```python
Sn = SymmetricGroup(25000)
g = Sn("...")
h = Sn("...")
t1 = Sn("...")
t2 = Sn("...")
dlog = (discrete_log(h,g))
m_ = (t2*(t1^-dlog))
m = hex(Permutation(m_).rank())
if "666c6167" in m:
    print(bytes.fromhex(m[m.index("666c6167"):]).decode())
```

We take the discrete log of (h,g) to find the private key; implementing the paper, we can decrypt.

Flag: flag{1_w1ll_n0t_34t_th3m_s4m_1_4m}
