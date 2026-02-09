MQV Code

Dependencies
- Chapter 3: `elptic_mul`, `elptic_sum_finish`

Listing 4.5: Ephemeral key generator

Python
```python
import secrets


def mqv_ephem(base):
    ephm = secrets.randbelow(base.order)
    Eph = Point()
    elptic_mul(Eph, base.Base, ephm, base.E, base.order)
    return ephm, Eph
```

MATLAB
```matlab
function [ephm, Eph] = mqv_ephem(base)
    ephm = mrand();
    Eph = point_init();
    Eph = elptic_mul(Eph, base.Base, ephm, base.E);
end
```

Listing 4.6: Associate value function

Python
```python
def avf(x, order):
    f = (order.bit_length() // 2) + 1
    mask = (1 << f) - 1
    z = x & mask
    z |= (1 << f)
    return z
```

MATLAB
```matlab
function z = avf(x, order)
    f = floor(log2(order));
    f = floor(f / 2) + 1;
    mask = uint64(0);
    for i = 0:f-1
        mask = bitset(mask, i+1, 1);
    end
    z = bitand(uint64(x), mask);
    z = bitset(z, f+1, 1);
end
```

Listing 4.7: Full ECC MQV shared secret

Python
```python
def mqv_share(my_sk, my_pk, my_ephm, my_ephm_pk,
              their_pk, their_ephm_pk, base):
    z = avf(my_ephm_pk.x, base.order)
    s = (my_ephm + z * my_sk) % base.order

    z2 = avf(their_ephm_pk.x, base.order)
    U = Point()
    elptic_mul(U, their_pk, z2, base.E, base.order)
    elptic_sum_finish(U, U, their_ephm_pk, base.E, base.order)

    elptic_mul(U, U, s, base.E, base.order)
    if base.cofactor > 1:
        elptic_mul(U, U, base.cofactor, base.E, base.order)
    return U.x
```

MATLAB
```matlab
function keyshare = mqv_share(my_sk, my_pk, my_ephm, my_ephm_pk, ...
    their_pk, their_ephm_pk, base)
    z = avf(my_ephm_pk.x, base.order);
    s = mod(my_ephm + z * my_sk, base.order);

    z2 = avf(their_ephm_pk.x, base.order);
    U = point_init();
    U = elptic_mul(U, their_pk, z2, base.E);
    U = elptic_sum_finish(U, U, their_ephm_pk, base.E);

    U = elptic_mul(U, U, s, base.E);
    if base.cofactor > 1
        U = elptic_mul(U, U, base.cofactor, base.E);
    end
    keyshare = U.x;
end
```

How to run (example)

Python
```python
skA, pkA = gen_key(b"alice", base)
skB, pkB = gen_key(b"bob", base)
eA, EpA = mqv_ephem(base)
eB, EpB = mqv_ephem(base)
ka = mqv_share(skA, pkA, eA, EpA, pkB, EpB, base)
kb = mqv_share(skB, pkB, eB, EpB, pkA, EpA, base)
print(ka == kb)
```

MATLAB
```matlab
[skA, pkA] = gen_key(uint8('alice'), base);
[skB, pkB] = gen_key(uint8('bob'), base);
[eA, EpA] = mqv_ephem(base);
[eB, EpB] = mqv_ephem(base);
ka = mqv_share(skA, pkA, eA, EpA, pkB, EpB, base);
kb = mqv_share(skB, pkB, eB, EpB, pkA, EpA, base);
disp(ka == kb);
```

Expected result
- Both sides compute the same keyshare.
