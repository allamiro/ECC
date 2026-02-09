MATLAB Implementation (Chapter 2)

This MATLAB version mirrors the Chapter 2 routines. It uses a stored modulus
and provides Legendre and square-root routines. The arithmetic is written for
regular numeric types; for very large primes you can replace the arithmetic
with Java BigInteger or Symbolic Math Toolbox equivalents.

```matlab
function demo_ch2()
    p = 23;
    minit(p);
    a = 2;
    r = msqrt(a);
    fprintf('sqrt: %d, check: %d\n', r, mod(r * r, p));
end

function minit(p)
    persistent modulus;
    modulus = p;
end

function p = mget()
    persistent modulus;
    p = modulus;
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
