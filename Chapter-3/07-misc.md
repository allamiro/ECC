Miscellaneous Routines

Listing 3.10: Random Point

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

Grouped view (optional)

Listings 3.10-3.12 cover random point generation, debugging output, and the header list.
