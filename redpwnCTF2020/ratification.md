**Ratification (crypto/27 solves)**

Like many others, we were able to cheese this challenge. Our method was to calculate <img src="https://render.githubusercontent.com/render/math?math=m^d"> by padding the message with zeroes. We can also abuse the fact that the given message is even, as shown:

<img src="https://render.githubusercontent.com/render/math?math=(m \cdot 2^65536)^d">
<img src="https://render.githubusercontent.com/render/math?math=((\frac{m}{2})^d \cdot 2^65537)^d">
<img src="https://render.githubusercontent.com/render/math?math=((\frac{m}{2})^d \cdot 2">

Multiplying this with <img src="https://render.githubusercontent.com/render/math?math=2^d">, we obtain:

<img src="https://render.githubusercontent.com/render/math?math=m^d \cdot 2">

Finally, we divide by 2:

<img src="https://render.githubusercontent.com/render/math?math=m^d">

All of the calculations were done modulo *n*, so we are able to submit this as a signature for the message. These calculations are implemented in the script below; credit to PMP for writing parts of the script.

```python
from pwn import *
from Crypto.Util.number import bytes_to_long, long_to_bytes
msg = bytes_to_long(b'redpwnCTF is a cybersecurity competition hosted by the redpwn CTF team.')
p = remote("2020.redpwnc.tf", 31752)
def get_sig(n):
    print()
    p.sendlineafter("[3] Exit\n", "1")
    p.sendlineafter("Message: ", long_to_bytes(n))
    p.recvline()
    return int(p.recvline())
ans=str((get_sig(msg*2**65536))*get_sig(2)//2)
p.sendlineafter("[3] Exit\n", "2")
p.sendlineafter("Message: ", b'redpwnCTF is a cybersecurity competition hosted by the redpwn CTF team.')
p.sendlineafter("Signature: ", ans)
p.recvline()
print(p.recvline())
```

Flag: flag{random_flags_are_secure-2504b7e69c65676367aef1d9658821030011f8968a640b504d320846ab5d5029b}
