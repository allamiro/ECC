Square Roots mod p

After confirming $a$ is a quadratic residue, the method depends on $p \bmod 4$:

1) If $p \equiv 3 \pmod{4}$, use the shortcut:
$$
x = a^{(p+1)/4} \bmod p
$$
This follows from Fermat's little theorem and is very fast.

2) If $p \equiv 1 \pmod{4}$, use Tonelli-Shanks:
   - Write $p - 1 = 2^e \cdot q$ with $q$ odd
   - Find a quadratic nonresidue n
   - Initialize:
     $y = n^q \bmod p$
     $x = a^{(q-1)/2} \bmod p$
     $b = a \cdot x^2 \bmod p$
     $x = a \cdot x \bmod p$
     r = e
   - Loop until b == 1:
     Find smallest $m$ such that $b^{2^m} \equiv 1 \pmod{p}$
     Update:
       $t = y^{2^{r-m-1}}$
       $y = t^2$
       $x = x \cdot t$
       $b = b \cdot y$
       r = m

This algorithm is reliable and works for any odd prime modulus.

Listing 2.5: Square Root mod p (Entry and Residue Check)

Python
```python
def sqrt_mod_entry(a, p):
    a = a % p
    if a == 0:
        return 0
    if legendre(a, p) != 1:
        return None
    return "use p3 mod4 or Tonelli-Shanks"
```

MATLAB
```matlab
function x = msqrt_entry(a)
    p = mget();
    a = mod(a, p);
    if a == 0
        x = 0;
        return;
    end
    if msqr(a) ~= 1
        x = [];
        return;
    end
    x = 'use p3 mod4 or Tonelli-Shanks';
end
```

Listing 2.6: Square Root mod p for $p \equiv 3 \pmod{4}$

Python
```python
def sqrt_mod_p3(a, p):
    return pow(a, (p + 1) // 4, p)
```

MATLAB
```matlab
function x = msqrt_p3(a)
    p = mget();
    x = modpow(a, (p + 1) / 4, p);
end
```

Listing 2.7: Square Root mod p (Tonelli-Shanks)

Python
```python
def sqrt_mod_tonelli(a, p):
    a = a % p
    if a == 0:
        return 0
    if legendre(a, p) != 1:
        return None

    q = p - 1
    e = 0
    while q % 2 == 0:
        e += 1
        q //= 2

    z = 2
    while legendre(z, p) != -1:
        z += 1

    y = pow(z, q, p)
    r = e
    x = pow(a, (q - 1) // 2, p)
    b = (a * x * x) % p
    x = (a * x) % p

    while b != 1:
        m = 1
        t = pow(b, 2, p)
        while t != 1:
            t = pow(t, 2, p)
            m += 1
            if m == r:
                return None

        t = pow(y, 1 << (r - m - 1), p)
        y = (t * t) % p
        x = (x * t) % p
        b = (b * y) % p
        r = m

    return x
```

MATLAB
```matlab
function x = msqrt_tonelli(a)
    p = mget();
    a = mod(a, p);
    if a == 0
        x = 0;
        return;
    end
    if msqr(a) ~= 1
        x = [];
        return;
    end
    if mod(p, 4) == 3
        x = modpow(a, (p + 1) / 4, p);
        return;
    end

    q = p - 1;
    e = 0;
    while mod(q, 2) == 0
        e = e + 1;
        q = q / 2;
    end

    z = 2;
    while msqr(z) ~= -1
        z = z + 1;
    end

    y = modpow(z, q, p);
    r = e;
    x = modpow(a, (q - 1) / 2, p);
    b = mod(a * x * x, p);
    x = mod(a * x, p);

    while b ~= 1
        m = 1;
        t = modpow(b, 2, p);
        while t ~= 1
            t = modpow(t, 2, p);
            m = m + 1;
            if m == r
                x = [];
                return;
            end
        end

        t = modpow(y, 2^(r - m - 1), p);
        y = mod(t * t, p);
        x = mod(x * t, p);
        b = mod(b * y, p);
        r = m;
    end
end
```

Listing 2.8: Small Integer Power mod p

Python
```python
def powi(a, i, p):
    if i < 0:
        a = mod_inv(a, p)
        return pow(a, -i, p)
    if i == 0:
        return 1
    return pow(a, i, p)
```

MATLAB
```matlab
function y = mpowi(a, i)
    p = mget();
    if i < 0
        a = modinv(a, p);
        y = modpow(a, -i, p);
    elseif i == 0
        y = 1;
    else
        y = modpow(a, i, p);
    end
end
```

Grouped view (optional): Full Square Root Function

Python
```python
def sqrt_mod(a, p):
    a = a % p
    if a == 0:
        return 0
    if legendre(a, p) != 1:
        return None

    if p % 4 == 3:
        return sqrt_mod_p3(a, p)

    return sqrt_mod_tonelli(a, p)
```

MATLAB
```matlab
function x = msqrt(a)
    p = mget();
    a = mod(a, p);
    if a == 0
        x = 0;
        return;
    end
    if msqr(a) ~= 1
        x = [];
        return;
    end
    if mod(p, 4) == 3
        x = msqrt_p3(a);
        return;
    end
    x = msqrt_tonelli(a);
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

function inv = modinv(a, n)
    a = mod(a, n);
    if a == 0
        error('division by zero in modinv');
    end
    [g, x] = egcd(a, n);
    if g ~= 1
        error('no modular inverse exists');
    end
    inv = mod(x, n);
end

function [g, x] = egcd(a, b)
    x0 = 1;
    x1 = 0;
    while b ~= 0
        q = floor(a / b);
        [a, b] = deal(b, a - q * b);
        [x0, x1] = deal(x1, x0 - q * x1);
    end
    g = a;
    x = x0;
end
```
