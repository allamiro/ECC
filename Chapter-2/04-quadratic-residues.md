Quadratic Residues

To solve $x^2 \equiv a \pmod{p}$, $a$ must be a quadratic residue modulo $p$. If no such $x$ exists,
a is a nonresidue and the square root does not exist.

The Legendre symbol classifies $a$:
 - $0$ if $p$ divides $a$
 - $1$ if $a$ is a quadratic residue mod $p$
 - $-1$ if $a$ is a nonresidue mod $p$

In practice, the Legendre symbol can be computed using:
$$
a^{(p-1)/2} \bmod p
$$
which returns $1$, $p-1$, or $0$. This is the test we use before attempting a square root.

Listing 2.4: Legendre Symbol

Python
```python
def legendre(a, p):
    a = a % p
    if a == 0:
        return 0
    ls = pow(a, (p - 1) // 2, p)
    return -1 if ls == p - 1 else 1
```

MATLAB
```matlab
function r = msqr(a)
    p = mget();
    a = mod(a, p);
    if a == 0
        r = 0;
        return;
    end
    ls = modpow(a, (p - 1) / 2, p);
    if ls == p - 1
        r = -1;
    else
        r = 1;
    end
end
```

Helper Routines (MATLAB)
```matlab
function y = modpow(a, e, n)
    y = 1;
    a = mod(a, n);
    while e > 0
        if mod(e, 2) == 1
            y = mod(y * a, n);
        end
        e = floor(e / 2);
        a = mod(a * a, n);
    end
end
```
