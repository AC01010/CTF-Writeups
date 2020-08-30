**4k-RSA (crypto/335 solves)**

The challenge description hints at having an *n* with multiple primes -- obviously, if the primes are too small, n can be easily factored, which makes computing for the private key *d* trivial.

I used alpertron to factor this during my first solve, which took about 10~15 minutes. Computing for phi = product of all (factor-1) and d = modinv(e,phi), we obtain pow(c,d,n), which is the flag.
