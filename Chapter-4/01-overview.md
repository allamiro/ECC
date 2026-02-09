Chapter 4 Overview

This chapter covers elliptic curve key exchange. Two parties exchange public keys
and compute the same shared secret using their private keys. We cover:
- Elliptic curve Diffie-Hellman (ECDH)
- The NIST Full ECC MQV algorithm (adds ephemeral keys)
- Hashing pass phrases into finite-field values

Key equation (ECDH):
$$
S = k_A B = k_B A = k_A k_B G
$$

How to use this chapter
- Each section contains a short explanation followed by Python and MATLAB listings.
- Run examples are provided near the end of each section.
