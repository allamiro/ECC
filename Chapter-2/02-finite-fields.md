Finite Fields and Elliptic Curve Groups

Finite fields are sets with a fixed number of elements where addition, subtraction,
multiplication, and division are all defined. For prime $p$, the field $\mathbb{F}_p$ is the
integers $\{0, 1, \ldots, p-1\}$ with arithmetic done modulo $p$. Prime modulus is essential because every
nonzero element has a multiplicative inverse.

This is like clock arithmetic: on a 12-hour clock, $9 + 6 \equiv 3 \pmod{12}$. For cryptography,
$p$ is huge, but the same modular rules apply.

Elliptic curves over F_p produce a finite set of points. These points form one or more
cyclic groups, and the factorization of the total point count determines possible subgroup
sizes. Picking a random point usually lands you in the largest subgroup; multiplying by
small factors can move the point into a subgroup of prime order, which is a common
requirement for cryptographic protocols.
