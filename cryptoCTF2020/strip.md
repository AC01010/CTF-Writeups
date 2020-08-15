**Strip**

**285 points, 11 solves, First Blood**

First, I noticed that a(n) was just a recursive function that generated the product of Mersenne numbers - {1, 3, 21, 315, 9765, 615195, 78129765...}. Knowing this, we can find a multiple of phi by multiplying the same Mersenne numbers together. Each factor of *n* can be represented in the form 2^n - 1 - it's corresponding factor in phi could be represented as (2^n - 1 - 1), which is just double the previous Mersenne number.

Knowing this, we can repeatedly attempt to decrypt *c* using an increasingly large phi. Searching for "CCTF{" in the decrypted *c*s, we find the flag.

```python
enc = 616666....
temp = 2
phi = 1
for i in range(1000):
    phi*=(2^temp)-1
    a = hex(pow(enc,inverse_mod(65539,phi*2),phi))
    if "434354467b" in a:
        print(bytes.fromhex(a[2:]))
        break
```

Flag:
**CCTF{R3arR4n9inG_7He_9Iv3n_eQu4t10n_T0_7h3_mUcH_MOrE_traCt4bLe_0n3}**
