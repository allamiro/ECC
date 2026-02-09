Modular Arithmetic Subroutines

The C implementation uses GMP and a consistent API style:
a = b (op) c mod n. The same structure carries over to Python and MATLAB.

Key ideas:
- Always reduce results modulo the field prime
- Protect against division by zero in modular division
- Use a single stored modulus when most routines share the same prime
- Provide both "with modulus" functions and "use stored modulus" functions

Core operations:
- Addition: (b + c) mod n
- Subtraction: (b - c) mod n
- Multiplication: (b * c) mod n
- Division: b * inv(c) mod n, where inv(c) exists when c != 0
- Negation: (-b) mod n

The stored-modulus approach mirrors the book's modulo.c style: initialize the modulus
once and then use lighter m* functions throughout the code.

Listing 2.1: Modular API (with modulus input)

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


def mod_rand(n):
    return secrets.randbelow(n)
```

MATLAB
```matlab
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

Listing 2.3: Initialization and Stored-Modulus API

Python
```python
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

Grouped view (optional)

The full modular API is the combination of Listings 2.1, 2.2, and 2.3 above.

Helper Routines (MATLAB)
```matlab
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
