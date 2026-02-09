Chapter 2 Overview

This chapter introduces finite fields over prime numbers and the core arithmetic
subroutines used throughout the book. The focus is on:
- Understanding fields and why primes give valid finite fields
- Seeing how elliptic curve points form cyclic groups over finite fields
- Building modular add/sub/mul/div/inv routines
- Deciding whether a modular square root exists
- Computing square roots using p mod 4 or Tonelli-Shanks

The code in this chapter is the foundation for later elliptic curve algorithms.

Elliptic curve context:
$$
y^2 + a_1xy + a_3y = x^3 + a_2x^2 + a_4x + a_6
$$
Over finite fields the book uses the simplified form:
$$
y^2 = x^3 + ax + b
$$
