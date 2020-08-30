**Worst-PW-Manager (crypto/66 solves)**

Reversing the code that generates the file name, we can see that we can easily reverse this operation by changing a few "+"s to "-"s. As a result, we're able to recover the password, making it trivial to recover the flag (the key).

Since RC4 is a reversable operation, we can test every single ascii character to see if the ciphertext matches up with the plaintext. If it does, we can then output a character in the flag. The following code, which contains many functions that are stolen from the challenge (credit to MrSlick), bruteforces each character in the key:

```python
import itertools
import string
import pathlib
import os
import glob

class KeyByteHolder(): # im paid by LoC, excuse the enterprise level code
    def __init__(self, num):
        assert num >= 0 and num < 256
        self.num = num

    def __repr__(self):
        return hex(self.num)[2:]

def rc4(text, key): # definitely not stolen from stackoverflow
    S = [i for i in range(256)]
    j = 0
    out = bytearray()

    #KSA Phase
    for i in range(256):
        j = (j + S[i] + key[i % len(key)].num) % 256
        S[i] , S[j] = S[j] , S[i]

    #PRGA Phase
    i = j = 0
    for char in text:
        i = ( i + 1 ) % 256
        j = ( j + S[i] ) % 256
        S[i] , S[j] = S[j] , S[i]
        out.append(ord(char) ^ S[(S[i] + S[j]) % 256])

    return out

def take(iterator, count):
    return [next(iterator) for _ in range(count)]

flag = itertools.cycle(bytearray("a", "utf-8"))
def generate_key(code):
    key = [KeyByteHolder(0)] * 8 # TODO: increase key length for more security?
    for i, c in enumerate(take(flag, 8)): # use top secret master password to encrypt all passwords
        key[i].num = code
    return key

def main(args):
  for i1 in range(100,408):
    fil = glob.glob(str(i1)+"_*")
    password=fil[0].split("_")[1].split(".")[0]
    masked_file_name = "".join([chr((((c - ord("0") - i) % 10) + ord("0")) * int(chr(c) not in string.ascii_lowercase) + (((c - ord("a") - i) % 26) + ord("a")) * int(chr(c) in string.ascii_lowercase)) for c, i in zip([ord(a) for a in password], range(0xffff))])
    f = open(fil[0], 'rb')
    hechs = (f.read().hex())
    for i in range(128):
      dec = (rc4((bytes.fromhex(hechs).decode("ISO-8859-1")), generate_key(i)))
      if (dec==bytearray(masked_file_name, "ISO-8859-1")):
        print(chr(i))


if __name__ == "__main__":
    import sys
    main(sys.argv)
```

In addition, the key length being 8 characters long also makes this encryption algorithm insecure. Looking at the output of the aforementioned script, we see that the flag is 43 characters long -- one '{' or '}' repeats every 43 characters. Therefore, we can take each character in the output and place it in the char[] that is to be our flag, remembering to skip positions in the array after every character. Doing this manually, I obtain the flag.

Flag: flag{crypto_is_stupid_and_python_is_stupid}
