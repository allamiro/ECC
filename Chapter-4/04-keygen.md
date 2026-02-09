Key Generation

We store public parameters (curve, base point, order, cofactor) in a base system
structure and generate keys by hashing a pass phrase into a private key, then
computing the public key as a point multiplication.

Dependencies
- Chapter 3: `Point`, `Curve`, `elptic_mul`
- Chapter 4: `hash_to_field`

Listing 4.2: Base system structure

Python
```python
class BaseSystem:
    def __init__(self, order, cofactor, base_point, curve):
        self.order = order
        self.cofactor = cofactor
        self.Base = base_point
        self.E = curve
```

MATLAB
```matlab
function B = base_system_make(order, cofactor, base_point, curve)
    B.order = order;
    B.cofactor = cofactor;
    B.Base = base_point;
    B.E = curve;
end
```

Listing 4.3: Key generation

Python
```python
def gen_key(passphrase_bytes, base):
    sk = hash_to_field(passphrase_bytes, base.order)
    PK = Point()
    elptic_mul(PK, base.Base, sk, base.E, base.order)
    return sk, PK
```

MATLAB
```matlab
function [sk, PK] = gen_key(passphrase_bytes, base)
    sk = hash_to_field(passphrase_bytes, base.order);
    PK = point_init();
    PK = elptic_mul(PK, base.Base, sk, base.E);
end
```

How to run (example)

Python
```python
# Example only; base parameters must be set correctly.
sk, PK = gen_key(b"my secret phrase", base)
print("sk =", sk)
point_printf("PK = ", PK)
```

MATLAB
```matlab
% Example only; base parameters must be set correctly.
[sk, PK] = gen_key(uint8('my secret phrase'), base);
disp(sk);
point_printf('PK = ', PK);
```

Expected result
- A private key integer and a matching public key point.
