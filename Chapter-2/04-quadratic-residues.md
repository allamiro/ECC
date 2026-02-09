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
