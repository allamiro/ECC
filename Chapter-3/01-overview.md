Chapter 3 Overview

This chapter introduces elliptic curve mathematics over finite fields and the core
subroutines needed for point addition, point multiplication, and data embedding.

The ordinary elliptic curve form used throughout is:
$$
y^2 = x^3 + a_4x + a_6
$$

Key ideas:
- Points form a group with the point at infinity as the identity
- Point addition uses a slope and reflection rule
- Point multiplication uses double-and-add
- Embedding data searches for x values with quadratic residues
