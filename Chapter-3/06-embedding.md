Embedding Data on a Curve

These routines assume Chapter 2 helpers: `msqr` (Legendre), `msqrt`, `madd`,
`mmul`, and `mneg`.

Listing 3.8: Computing $f(x)$

Python
```python
def fofx(x, E, p):
    t1 = mod_mul(x, x, p)
    t1 = mod_mul(t1, x, p)
    t2 = mod_mul(E.a4, x, p)
    return mod_add(mod_add(t1, t2, p), E.a6, p)
```

MATLAB
```matlab
function f = fofx(x, E)
    t1 = mmul(x, x);
    t1 = mmul(t1, x);
    t2 = mmul(E.a4, x);
    f = madd(madd(t1, t2), E.a6);
end
```

Listing 3.9: Embedding Data (Find Points)

Python
```python
def elptic_embed(P1, P2, x, E, p):
    one = 1
    P1.x = x
    while True:
        f = fofx(P1.x, E, p)
        if legendre(f, p) > 0:
            break
        P1.x = mod_add(P1.x, one, p)

    P2.x = P1.x
    P1.y = sqrt_mod(f, p)
    P2.y = mod_neg(P1.y, p)

    if P2.y < P1.y:
        P1.y, P2.y = P2.y, P1.y
```

MATLAB
```matlab
function [P1, P2] = elptic_embed(P1, P2, x, E)
    one = 1;
    P1.x = x;
    done = false;
    while ~done
        f = fofx(P1.x, E);
        if msqr(f) > 0
            done = true;
        else
            P1.x = madd(P1.x, one);
        end
    end

    P2.x = P1.x;
    P1.y = msqrt(f);
    P2.y = mneg(P1.y);
    if P2.y < P1.y
        t = P1.y;
        P1.y = P2.y;
        P2.y = t;
    end
end
```

Grouped view (optional)

Listing 3.8 computes the curve RHS and Listing 3.9 searches for a valid point.
