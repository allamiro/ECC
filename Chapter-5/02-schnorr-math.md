Schnorr Digital Signature (Math)

Let the curve be:
$$
y^2 = x^3 + a_4x + a_6
$$
with base point $B$ of prime order $n$. Private key is $s_p$ and public key is
$$
P_p = s_p B
$$

Signing steps:
1) Pick random $k \in [1, n-1]$ and compute
$$
Q = kB
$$
2) Hash the point with the message:
$$
e = \text{hash}(Q.x \Vert Q.y \Vert M) \bmod n
$$
3) Compute
$$
s = k - e\cdot s_p \bmod n
$$
Signature is the pair $(Q, s)$.

Verification:
Compute $e$ again and check:
$$
Q \stackrel{?}{=} sB + eP_p
$$

Exercise 5.1
In the verification equation, how many times does the random value $k$ appear?
