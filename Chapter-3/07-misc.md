Miscellaneous Routines

Listing 3.10: Random Point

This picks a random x value and embeds it on the curve. The low bit selects which
of the two y values is returned.

Python
```python
def point_rand(P, E, p):
    r = mod_rand(p)
    mP = point_init()
    if r & 1:
        elptic_embed(P, mP, r, E, p)
    else:
        elptic_embed(mP, P, r, E, p)
```

MATLAB
```matlab
function P = point_rand(P, E)
    r = mrand();
    mP = point_init();
    if bitget(r, 1)
        [P, mP] = elptic_embed(P, mP, r, E);
    else
        [mP, P] = elptic_embed(mP, P, r, E);
    end
end
```

Listing 3.11: Print a Point

Simple console output for debugging and verification.

Python
```python
def point_printf(label, P):
    print(f"{label} ({P.x}, {P.y})")
```

MATLAB
```matlab
function point_printf(str, P)
    fprintf('%s', str);
    fprintf('(%d, %d)\n', P.x, P.y);
end
```

Listing 3.12: Header Function Definitions (Summary)

Python
```python
# point_init, point_clear, curve_init, curve_clear
# point_copy, test_point, fofx
# elptic_sum, elptic_embed, point_printf
# elptic_mul, point_rand
```

MATLAB
```matlab
% point_init, point_clear, curve_init, curve_clear
% point_copy, test_point, fofx
% elptic_sum, elptic_embed, point_printf
% elptic_mul, point_rand
```

How to run (example)

Python
```python
p = 43
E = Curve(a4=23, a6=-1)
P = point_init()
point_rand(P, E, p)
point_printf("Rand = ", P)
```

MATLAB
```matlab
minit(43);
E = curve_make(23, -1);
P = point_init();
P = point_rand(P, E);
point_printf('Rand = ', P);
```

Expected result
- A random valid curve point printed as `Rand = (x, y)`.

Grouped view (optional)

Listings 3.10-3.12 cover random point generation, debugging output, and the header list.
