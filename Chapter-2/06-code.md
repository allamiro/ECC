Chapter 2 Listings - Python and MATLAB

This file contains the Chapter 2 C listings converted into Python and MATLAB, with
both languages shown together for each section.

Notes:
- Python uses built-in big integers.
- MATLAB code uses standard numeric types; for large primes use Symbolic Math
  Toolbox or Java BigInteger.

Listing 2.1-2.3: Modular API and Initialization

Python
```python
import secrets


def mod_add(b, c, n):
    return (b + c) % n


def mod_sub(b, c, n):
    return (b - c) % n


def mod_mul(b, c, n):
    return (b * c) % n


def mod_inv(a, n):
    a = a % n
    if a == 0:
        raise ZeroDivisionError("division by zero in mod_inv")
    return pow(a, -1, n)


def mod_div(b, c, n):
    return (b * mod_inv(c, n)) % n


def mod_neg(b, n):
    return (-b) % n


class ModCtx:
    def __init__(self, p):
        if p <= 2 or p % 2 == 0:
            raise ValueError("modulus must be an odd prime")
        self.p = p

    def add(self, a, b):
        return (a + b) % self.p

    def sub(self, a, b):
        return (a - b) % self.p

    def mul(self, a, b):
        return (a * b) % self.p

    def div(self, a, b):
        return (a * mod_inv(b, self.p)) % self.p

    def neg(self, a):
        return (-a) % self.p

    def mrand(self):
        return secrets.randbelow(self.p)
```

MATLAB
```matlab
function minit(p)
    persistent modulus;
    modulus = p;
end

function p = mget()
    persistent modulus;
    p = modulus;
end

function c = mod_add(b, c, n)
    c = mod(b + c, n);
end

function c = mod_sub(b, c, n)
    c = mod(b - c, n);
end

function c = mod_mul(b, c, n)
    c = mod(b * c, n);
end

function c = mod_div(b, c, n)
    c = mod(b * modinv(c, n), n);
end

function c = mod_neg(b, n)
    c = mod(-b, n);
end

function c = madd(a, b)
    p = mget();
    c = mod(a + b, p);
end

function c = msub(a, b)
    p = mget();
    c = mod(a - b, p);
end

function c = mmul(a, b)
    p = mget();
    c = mod(a * b, p);
end

function c = mdiv(a, b)
    p = mget();
    c = mod(a * modinv(b, p), p);
end

function c = mneg(a)
    p = mget();
    c = mod(-a, p);
end
```

Listing 2.2: Modular Division (Error on Zero)

Python
```python
def mod_div_checked(b, c, n):
    if c % n == 0:
        raise ZeroDivisionError("division by zero in mod_div")
    return (b * mod_inv(c, n)) % n
```

MATLAB
```matlab
function c = mod_div_checked(b, c, n)
    if mod(c, n) == 0
        error('division by zero in mod_div');
    end
    c = mod(b * modinv(c, n), n);
end
```

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

Listing 2.5-2.7: Square Root mod p (Tonelli-Shanks)

Python
```python
def sqrt_mod(a, p):
    a = a % p
    if a == 0:
        return 0
    if legendre(a, p) != 1:
        return None

    if p % 4 == 3:
        return pow(a, (p + 1) // 4, p)

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
