**blecc**

*199 points, 146 solves*

```python
E = EllipticCurve(GF(17459102747413984477),[2,3])
g = E(15579091807671783999, 4313814846862507155)
d = E(8859996588597792495, 2628834476186361781)
print((b"flag{"+bytes.fromhex(hex(g.discrete_log(d))[2:])+b"}").decode())
```

This is a simple ECDH challenge; all we have to do is to take the discrete log of the given point to find the private key.

Flag: flag{m1n1_3cc}
