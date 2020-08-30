**Pseudo-Key (crypto/447 solves)**

First, we can decode the key: "REDPWWWNCTF".

This is really just an implementation of the vigenere cipher.

Using dcode.fr, I can decode the ciphertext "zXjjaooXrljlhrXgaufXtwvXshaqzbXljtyut", with the underscores converted to Xs -- dcode.fr strips the underscores when computing the index of each character in the string.

Output: iTguessKpseudoIkeysVareTpseudoVsecure

Flag: flag{i_guess_pseudo_keys_are_pseudo_secure}
