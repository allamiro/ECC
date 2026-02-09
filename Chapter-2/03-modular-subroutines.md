Modular Arithmetic Subroutines

The C implementation uses GMP and a consistent API style:
a = b (op) c mod n. The same structure carries over to Python and MATLAB.

Key ideas:
- Always reduce results modulo the field prime
- Protect against division by zero in modular division
- Use a single stored modulus when most routines share the same prime
- Provide both "with modulus" functions and "use stored modulus" functions

Core operations:
- Addition: (b + c) mod n
- Subtraction: (b - c) mod n
- Multiplication: (b * c) mod n
- Division: b * inv(c) mod n, where inv(c) exists when c != 0
- Negation: (-b) mod n

The stored-modulus approach mirrors the book's modulo.c style: initialize the modulus
once and then use lighter m* functions throughout the code.
