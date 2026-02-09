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
