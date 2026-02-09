Point Multiplication

Listing 3.7: Point Multiplication (Double-and-Add)

This uses repeated doubling and conditional addition based on the bits of $k$.

Python
```python
def elptic_mul(Q, P, k, E, p):
    R = point_copy(P)
    j = k.bit_length() - 2
    while j >= 0:
        elptic_sum_finish(R, R, R, E, p)
        if (k >> j) & 1:
            elptic_sum_finish(R, R, P, E, p)
        j -= 1
    Q.x, Q.y = R.x, R.y
```

MATLAB
```matlab
function Q = elptic_mul(Q, P, k, E)
    R = point_copy(P);
    j = floor(log2(k)) - 1;
    while j >= 0
        R = elptic_sum_finish(R, R, R, E);
        if bitget(k, j + 1)
            R = elptic_sum_finish(R, R, P, E);
        end
        j = j - 1;
    end
    Q = point_copy(R);
end
```

How to run (example)

Python
```python
p = 43
E = Curve(a4=23, a6=-1)
P = Point(3, 10)
Q = Point()
elptic_mul(Q, P, 7, E, p)
print("7P =", Q.x, Q.y)
```

MATLAB
```matlab
minit(43);
E = curve_make(23, -1);
P = point_make(3, 10);
Q = point_init();
Q = elptic_mul(Q, P, 7, E);
point_printf('7P = ', Q);
```

Expected result
- A single point printed as `7P = (x, y)` on the curve.

Grouped view (optional)

Listing 3.7 uses point addition for doubling and adding, matching the double-and-add
diagram from the text.
