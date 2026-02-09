Chapter 5 Summary

- Schnorr signatures produce a point $Q$ and scalar $s$ using a random nonce and hash.
- Verification recomputes the hash and checks $Q = sB + eP_p$.
- ECDSA produces two scalars $(c, d)$ derived from $R = kB$ and the message hash.
- Verification rebuilds $R'$ and checks that $R'.x$ matches $c$.
