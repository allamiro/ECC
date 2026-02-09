MQV Algorithm (Math)

MQV adds ephemeral keys to provide forward secrecy. Notation:
- Static keys: $(k_{s,A}, P_{s,A})$ and $(k_{s,B}, P_{s,B})$
- Ephemeral keys: $(k_{e,A}, P_{e,A})$ and $(k_{e,B}, P_{e,B})$

Public keys:
$$
P_{s,A} = k_{s,A}G,\quad P_{e,A} = k_{e,A}G
$$
$$
P_{s,B} = k_{s,B}G,\quad P_{e,B} = k_{e,B}G
$$

Associate value function:
$$
\operatorname{avf}(x) = x \bmod 2^{\lceil \log_2(n)/2 \rceil} + 2^{\lceil \log_2(n)/2 \rceil}
$$
where $n$ is the order of $G$.

Alice computes:
$$
s_A = k_{e,A} + \operatorname{avf}(P_{e,A}.x)\,k_{s,A}
$$
$$
U_B = P_{e,B} + \operatorname{avf}(P_{e,B}.x)\,P_{s,B}
$$
$$
z = (s_A U_B).x
$$

Bob computes the symmetric expression and gets the same $z$.

Exercise 4.2
How many point multiplications are required for one side?
