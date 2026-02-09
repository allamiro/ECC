NIST ECDSA (Math)

Let the base point be $B$ of order $n$, private key $s_p$ and public key $P_p = s_p B$.

Signing:
1) Hash the message:
$$
e = \text{hash}(M) \bmod n
$$
2) Pick random $k$ and compute:
$$
R = kB
$$
3) Reduce the x-coordinate:
$$
c = R.x \bmod n
$$
4) Compute:
$$
d = k^{-1}(e + c s_p) \bmod n
$$
Signature is the pair $(c, d)$.

Verification:
1) Compute $e' = \text{hash}(M) \bmod n$.
2) Compute:
$$
h = d^{-1} \bmod n
$$
$$
h_1 = e' h \bmod n,\quad h_2 = c h \bmod n
$$
3) Rebuild:
$$
R' = h_1 B + h_2 P_p
$$
Verify:
$$
R'.x \stackrel{?}{=} c
$$

Exercise 5.2
Is $n$ the field prime or the prime order of the base point?
