Schnorr Code

Dependencies
- Chapter 3: `Point`, `elptic_mul`, `elptic_sum_finish`
- Chapter 4: `hash_to_field` (or `hash` equivalent)
- Chapter 2: modular arithmetic (`mod_add`, `mod_sub`, `mod_mul`, `mod_div`)

Listing 5.1: Schnorr signature structure

What it does
- Holds the signature point `Q` and scalar `s`.

Python
```python
class SchnorrSig:
    def __init__(self):
        self.Q = Point()
        self.s = 0
```

MATLAB
```matlab
function sig = schnorr_make()
    sig.Q = point_init();
    sig.s = 0;
end
```

Listing 5.2: Schnorr sign

What it does
- Picks random nonce `k` and computes `Q = kB`.
- Hashes `Q.x || Q.y || M` to get `e`.
- Computes `s = k - e*sk (mod n)`.

Python
```python
import secrets


def schnorr_sign(sk, msg_bytes, base):
    sig = SchnorrSig()
    k = secrets.randbelow(base.order)
    elptic_mul(sig.Q, base.Base, k, base.E, base.order)

    qx = sig.Q.x.to_bytes((sig.Q.x.bit_length() + 7) // 8, "little")
    qy = sig.Q.y.to_bytes((sig.Q.y.bit_length() + 7) // 8, "little")
    e = hash_to_field(qx + qy + msg_bytes, base.order)

    sig.s = (k - e * sk) % base.order
    return sig
```

MATLAB
```matlab
function sig = schnorr_sign(sk, msg_bytes, base)
    sig = schnorr_make();
    k = mrand();
    sig.Q = elptic_mul(sig.Q, base.Base, k, base.E);

    qx = int_to_bytes_le(sig.Q.x);
    qy = int_to_bytes_le(sig.Q.y);
    e = hash_to_field([qx, qy, msg_bytes], base.order);

    sig.s = mod(k - e * sk, base.order);
end

function out = int_to_bytes_le(x)
    out = uint8([]);
    while x > 0
        out = [out, uint8(bitand(x, 255))]; %#ok<AGROW>
        x = floor(x / 256);
    end
end
```

Listing 5.3: Schnorr verify

What it does
- Recomputes `e` from `Q` and the message.
- Checks that `Q == sB + ePp`.

Python
```python
def schnorr_verify(sig, pk, msg_bytes, base):
    qx = sig.Q.x.to_bytes((sig.Q.x.bit_length() + 7) // 8, "little")
    qy = sig.Q.y.to_bytes((sig.Q.y.bit_length() + 7) // 8, "little")
    e = hash_to_field(qx + qy + msg_bytes, base.order)

    U = Point()
    V = Point()
    elptic_mul(U, base.Base, sig.s, base.E, base.order)
    elptic_mul(V, pk, e, base.E, base.order)
    elptic_sum_finish(U, U, V, base.E, base.order)
    return (U.x == sig.Q.x) and (U.y == sig.Q.y)
```

MATLAB
```matlab
function ok = schnorr_verify(sig, pk, msg_bytes, base)
    qx = int_to_bytes_le(sig.Q.x);
    qy = int_to_bytes_le(sig.Q.y);
    e = hash_to_field([qx, qy, msg_bytes], base.order);

    U = point_init();
    V = point_init();
    U = elptic_mul(U, base.Base, sig.s, base.E);
    V = elptic_mul(V, pk, e, base.E);
    U = elptic_sum_finish(U, U, V, base.E);
    ok = (U.x == sig.Q.x) && (U.y == sig.Q.y);
end
```

How to run (example)

Python
```python
sig = schnorr_sign(sk, b"hello", base)
print(schnorr_verify(sig, pk, b"hello", base))
```

MATLAB
```matlab
sig = schnorr_sign(sk, uint8('hello'), base);
disp(schnorr_verify(sig, pk, uint8('hello'), base));
```

Expected result
- Verification returns `True` for the original message and `False` for any modified message.
