Diffie-Hellman Shared Secret

Once both sides have exchanged public keys, each side multiplies the other person's
public key by their own private key to get the same shared point.

Dependencies
- Chapter 3: `elptic_mul`

Listing 4.4: Diffie-Hellman shared key

Python
```python
def diffie_hellman_shared(my_sk, their_pk, curve, p):
    tmp = Point()
    elptic_mul(tmp, their_pk, my_sk, curve, p)
    return tmp.x
```

MATLAB
```matlab
function keyshare = diffie_hellman_shared(my_sk, their_pk, curve)
    tmp = point_init();
    tmp = elptic_mul(tmp, their_pk, my_sk, curve);
    keyshare = tmp.x;
end
```

How to run (example)

Python
```python
my_share = diffie_hellman_shared(skA, pkB, base.E, base.order)
their_share = diffie_hellman_shared(skB, pkA, base.E, base.order)
print(my_share == their_share)
```

MATLAB
```matlab
my_share = diffie_hellman_shared(skA, pkB, base.E);
their_share = diffie_hellman_shared(skB, pkA, base.E);
disp(my_share == their_share);
```

Expected result
- Both sides compute the same integer keyshare.
