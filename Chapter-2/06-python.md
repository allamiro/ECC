Python Implementation (Chapter 2)

Below is a self-contained Python version of the Chapter 2 modular arithmetic routines.
It provides both "with modulus" functions and a stored-modulus context like the C code.

```python
import secrets


def egcd(a, b):
    while b:
        a, b = b, a % b
    return a


def mod_inv(a, n):
    a = a % n
    if a == 0:
        raise ZeroDivisionError("division by zero in mod_inv")
    # Python 3.8+ supports pow(a, -1, n)
    return pow(a, -1, n)


def mod_add(b, c, n):
    return (b + c) % n


def mod_sub(b, c, n):
    return (b - c) % n


def mod_mul(b, c, n):
    return (b * c) % n


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

    def powi(self, a, i):
        if i < 0:
            a = mod_inv(a, self.p)
            return pow(a, -i, self.p)
        if i == 0:
            return 1
        return pow(a, i, self.p)

    def legendre(self, a):
        a = a % self.p
        if a == 0:
            return 0
        ls = pow(a, (self.p - 1) // 2, self.p)
        return -1 if ls == self.p - 1 else 1

    def sqrt(self, a):
        a = a % self.p
        if a == 0:
            return 0
        if self.legendre(a) != 1:
            return None

        if self.p % 4 == 3:
            return pow(a, (self.p + 1) // 4, self.p)

        # Tonelli-Shanks
        q = self.p - 1
        e = 0
        while q % 2 == 0:
            e += 1
            q //= 2

        while True:
            z = secrets.randbelow(self.p)
            if z > 1 and self.legendre(z) == -1:
                break

        y = pow(z, q, self.p)
        r = e
        x = pow(a, (q - 1) // 2, self.p)
        b = (a * x * x) % self.p
        x = (a * x) % self.p

        while b != 1:
            m = 1
            t = pow(b, 2, self.p)
            while t != 1:
                t = pow(t, 2, self.p)
                m += 1
                if m == r:
                    return None

            t = pow(y, 1 << (r - m - 1), self.p)
            y = (t * t) % self.p
            x = (x * t) % self.p
            b = (b * y) % self.p
            r = m

        return x


if __name__ == "__main__":
    p = 23
    ctx = ModCtx(p)
    a = 2
    root = ctx.sqrt(a)
    print("sqrt:", root, "check:", (root * root) % p)
```
