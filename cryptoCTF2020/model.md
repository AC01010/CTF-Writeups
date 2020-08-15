**Model**

**112 points, 40 solves**

First, I modified a PoW script found [here](http://hkopp.github.io/2019/08/writeup-crypto-ctf) in order to access the challenge. This was a classic RSA implementation, except it was possible to use the public exponent as the private exponent. Since *e* was not released as part of the public key, you could instead take the encrypted flag and send it through the encryption function as bytes. 

```python
import uuid
from pwn import *
import hashlib
import time
import os
start = time.time()
c = remote('04.cr.yp.toc.tf', 8001)
line = c.recvline().decode("utf-8").split(" ")
hash = (line[8].split("(")[0])
val = (line[10])
len = int(line[-1].strip())
print(hash,val,len)
def random_ascii_generator():
    return ((uuid.uuid4().hex.encode('ascii'))+(uuid.uuid4().hex.encode('ascii')))[:len]
algo_matching = {"sha1": hashlib.sha1,
        "md5": hashlib.md5,
        "sha224": hashlib.sha224,
        "sha256": hashlib.sha256,
        "sha384": hashlib.sha384,
        "sha512": hashlib.sha512
        }
candidate_hash = ""
while not candidate_hash[-6:] == val:
    candidate_string = random_ascii_generator()
    candidate_hash = algo_matching[hash](candidate_string).hexdigest()
    if time.time()-start>15:
        os.execl(sys.executable, sys.executable, *sys.argv)
print("[*] PoW found")
print(candidate_string)
c.sendline(candidate_string)
c.recvuntil("[Q]uit\n")
c.sendline("c")
num = hex(int(c.recvline().strip().split(b" ")[-1]))[2:]
a = bytes.fromhex(num)
c.recvuntil("[Q]uit\n")
c.sendline("t")
c.sendline(a)
print(c.recvline())
print(c.recvline())
```

Flag: 
**CCTF{7He_mA1n_iD34_0f_pUb1iC_key_cryPto9raphy_iZ_tHa7_It_l3ts_y0u_puBli5h_4N_pUbL!c_k3y_wi7hOuT_c0mprOmi5InG_y0Ur_5ecr3T_keY}**
