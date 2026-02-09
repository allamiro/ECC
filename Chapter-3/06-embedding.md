Embedding Data on a Curve

These routines assume Chapter 2 helpers.

- Python: `legendre`, `sqrt_mod`, `mod_add`, `mod_mul`, `mod_neg`
- MATLAB: `msqr`, `msqrt`, `madd`, `mmul`, `mneg`

Listing 3.8: Computing $f(x)$

This computes the right-hand side of the curve equation:
$$
f(x) = x^3 + a_4x + a_6
$$

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

This increments $x$ until $f(x)$ is a quadratic residue, then returns the two points
with $y = \pm \sqrt{f(x)}$.

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

How to run (example)

Python
```python
p = 43
E = Curve(a4=23, a6=-1)
P1 = Point()
P2 = Point()
elptic_embed(P1, P2, 27, E, p)
print("P1 =", P1.x, P1.y)
print("P2 =", P2.x, P2.y)
```

MATLAB
```matlab
minit(43);
E = curve_make(23, -1);
P1 = point_init();
P2 = point_init();
[P1, P2] = elptic_embed(P1, P2, 27, E);
point_printf('P1 = ', P1);
point_printf('P2 = ', P2);
```

Expected result
- Two points with the same x value and opposite y values.
Grouped view (optional)

Listing 3.8 computes the curve RHS and Listing 3.9 searches for a valid point.
