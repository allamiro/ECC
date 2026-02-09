Point Multiplication

Listing 3.7: Point Multiplication (Double-and-Add)

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

Grouped view (optional)

Listing 3.7 uses point addition for doubling and adding, matching the double-and-add
diagram from the text.
