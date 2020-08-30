**Anti-Textbook (web/69 solves)**

This challenge gave you an RSA public key, and hinted at `certificate-transparency.org`. 

First, I used openssl to create an RSA public key pem file out of the given public key:

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuffuWhYrpTW8cdcAWUwe
T8oZYCp/8pKPYj4eZ3pd7mhYoCkSSeqZ5e+L33O38SoMANogM1NBayYlumOcPxC/
C9PHMF6AlaLDH+yX/Fg+a055m0O7+5pJNUVuRn9z7aYhhubnRyjk2cVTHLmOHqK9
FPM1QBBdouddMgZYE6plaBdBIMwQ8txuZQs6t862zJfA0/cgT47TtiTNkouHkAuT
VXBPcbM5pXIu7MoflJrUjQ0ljuOIFgXQ7wCFusXrIpvuVpqLzRvTD69GA7Cj0Dt9
ij7KPrBFM2jFyR8vnm5w+T6sGafXgJEEj0sLmbIReWcNeyHC2Tl9OniyMEqPeLsZ
oQIDAQAB
-----END PUBLIC KEY-----
```

This is the given subject public key information. I can then use the base64 encoded public key, convert it to hex, and take the sha256 hash of the bytes. 

I used [this](http://www.fileformat.info/tool/hash.htm?hex=30820122300d06092a864886f70d01010105000382010f003082010a0282010100b9f7ee5a162ba535bc71d700594c1e4fca19602a7ff2928f623e1e677a5dee6858a0291249ea99e5ef8bdf73b7f12a0c00da203353416b2625ba639c3f10bf0bd3c7305e8095a2c31fec97fc583e6b4e799b43bbfb9a4935456e467f73eda62186e6e74728e4d9c5531cb98e1ea2bd14f33540105da2e75d32065813aa6568174120cc10f2dc6e650b3ab7ceb6cc97c0d3f7204f8ed3b624cd928b87900b9355704f71b339a5722eecca1f949ad48d0d258ee3881605d0ef0085bac5eb229bee569a8bcd1bd30faf4603b0a3d03b7d8a3eca3eb0453368c5c91f2f9e6e70f93eac19a7d78091048f4b0b99b21179670d7b21c2d9397d3a78b2304a8f78bb19a10203010001) website to hash the raw bytes for me, and the resulting hash was `9db105389dd81cfb4b59ff1a4c0670c630b1800e542323111d5c5cb9af72031f`.

Searching for the hash on [crt.sh](https://crt.sh/?spkisha256=9db105389dd81cfb4b59ff1a4c0670c630b1800e542323111d5c5cb9af72031f) results in the website:

[https://oa4gio7glypwggb9iu3rh8mrc87tnjbs.flag.ga/](https://oa4gio7glypwggb9iu3rh8mrc87tnjbs.flag.ga/)

Flag: flag{c3rTific4t3_7r4n5pArAncY_fTw}
