ECDSA Code

Dependencies
- Chapter 3: `Point`, `elptic_mul`, `elptic_sum_finish`
- Chapter 4: `hash_to_field` (or `hash` equivalent)
- Chapter 2: modular arithmetic (`mod_add`, `mod_sub`, `mod_mul`, `mod_div`)

Listing 5.6: ECDSA structure

What it does
- Holds the signature scalars `c` and `d`.

Python
```python
class ECDSASig:
    def __init__(self):
        self.c = 0
        self.d = 0
```

MATLAB
```matlab
function sig = ecdsa_make()
    sig.c = 0;
    sig.d = 0;
end
```

Listing 5.7: ECDSA sign

What it does
- Hashes the message to `e`.
- Picks random `k` and computes `R = kB`.
- Computes `c = R.x mod n` and `d = k^{-1}(e + c*sk) mod n`.

Python
```python
import secrets


def ecdsa_sign(sk, msg_bytes, base):
    sig = ECDSASig()
    e = hash_to_field(msg_bytes, base.order)
    k = secrets.randbelow(base.order)
    R = Point()
    elptic_mul(R, base.Base, k, base.E, base.order)
    sig.c = R.x % base.order
    sig.d = (e + sig.c * sk) * pow(k, -1, base.order)
    sig.d %= base.order
    return sig
```

MATLAB
```matlab
function sig = ecdsa_sign(sk, msg_bytes, base)
    sig = ecdsa_make();
    e = hash_to_field(msg_bytes, base.order);
    k = mrand();
    R = point_init();
    R = elptic_mul(R, base.Base, k, base.E);
    sig.c = mod(R.x, base.order);
    sig.d = mod((e + sig.c * sk) * modinv(k, base.order), base.order);
end
```

Listing 5.8: ECDSA verify

What it does
- Recomputes the message hash.
- Builds $R' = h_1 B + h_2 P_p$ and checks $R'.x == c$.

Python
```python
def ecdsa_verify(sig, pk, msg_bytes, base):
    e = hash_to_field(msg_bytes, base.order)
    h = pow(sig.d, -1, base.order)
    h1 = (e * h) % base.order
    h2 = (sig.c * h) % base.order
    S = Point()
    T = Point()
    elptic_mul(S, base.Base, h1, base.E, base.order)
    elptic_mul(T, pk, h2, base.E, base.order)
    elptic_sum_finish(S, S, T, base.E, base.order)
    return (S.x % base.order) == sig.c
```

MATLAB
```matlab
function ok = ecdsa_verify(sig, pk, msg_bytes, base)
    e = hash_to_field(msg_bytes, base.order);
    h = modinv(sig.d, base.order);
    h1 = mod(e * h, base.order);
    h2 = mod(sig.c * h, base.order);
    S = point_init();
    T = point_init();
    S = elptic_mul(S, base.Base, h1, base.E);
    T = elptic_mul(T, pk, h2, base.E);
    S = elptic_sum_finish(S, S, T, base.E);
    ok = (mod(S.x, base.order) == sig.c);
end
```

How to run (example)

Python
```python
sig = ecdsa_sign(sk, b"hello", base)
print(ecdsa_verify(sig, pk, b"hello", base))
```

MATLAB
```matlab
sig = ecdsa_sign(sk, uint8('hello'), base);
disp(ecdsa_verify(sig, pk, uint8('hello'), base));
```

Expected result
- Verification returns `True` for the original message and `False` for any modified message.
