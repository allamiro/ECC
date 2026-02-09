Square Roots mod p

After confirming a is a quadratic residue, the method depends on p mod 4:

1) If p % 4 == 3, use the shortcut:
   x = a^((p+1)/4) mod p
This follows from Fermat's little theorem and is very fast.

2) If p % 4 == 1, use Tonelli-Shanks:
   - Write p - 1 = 2^e * q with q odd
   - Find a quadratic nonresidue n
   - Initialize:
     y = n^q mod p
     x = a^((q-1)/2) mod p
     b = a * x^2 mod p
     x = a * x mod p
     r = e
   - Loop until b == 1:
     Find smallest m such that b^(2^m) == 1
     Update:
       t = y^(2^(r-m-1))
       y = t^2
       x = x * t
       b = b * y
       r = m

This algorithm is reliable and works for any odd prime modulus.
