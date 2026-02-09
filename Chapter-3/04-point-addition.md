Point Addition

These routines assume modular arithmetic helpers from Chapter 2:
`mod_add`, `mod_sub`, `mod_mul`, `mod_div`, and `mod_neg`.

Listing 3.4: Point Addition - Test for Infinity

Python
```python
def elptic_sum(R, P, Q, E, p):
    if test_point(P):
        R.x, R.y = Q.x, Q.y
        return
    if test_point(Q):
        R.x, R.y = P.x, P.y
        return
    # Continue with slope calculation (Listing 3.5)
```

MATLAB
```matlab
function R = elptic_sum(R, P, Q, E)
    if test_point(P)
        R = point_copy(Q);
        return;
    end
    if test_point(Q)
        R = point_copy(P);
        return;
    end
    % Continue with slope calculation (Listing 3.5)
end
```

Listing 3.5: Point Addition - Computing $\lambda$

Python
```python
def elptic_lambda(P, Q, E, p):
    t1 = mod_mul(P.x, P.x, p)
    t2 = mod_mul(P.x, Q.x, p)
    t3 = mod_mul(Q.x, Q.x, p)
    top = mod_add(mod_add(mod_add(t1, t2, p), t3, p), E.a4, p)
    bottom = mod_add(P.y, Q.y, p)

    if bottom == 0:
        dx = mod_sub(Q.x, P.x, p)
        if dx == 0:
            return None  # point at infinity
        top = mod_sub(Q.y, P.y, p)
        bottom = dx

    return mod_div(top, bottom, p)
```

MATLAB
```matlab
function lmbda = elliptic_lambda(P, Q, E)
    p = mget();
    t1 = mmul(P.x, P.x);
    t2 = mmul(P.x, Q.x);
    t3 = mmul(Q.x, Q.x);
    top = madd(madd(madd(t1, t2), t3), E.a4);
    bottom = madd(P.y, Q.y);

    if bottom == 0
        dx = msub(Q.x, P.x);
        if dx == 0
            lmbda = [];
            return;
        end
        top = msub(Q.y, P.y);
        bottom = dx;
    end

    lmbda = mdiv(top, bottom);
end
```

Listing 3.6: Point Addition - $x_3$ and $y_3$

Python
```python
def elptic_sum_finish(R, P, Q, E, p):
    lmbda = elptic_lambda(P, Q, E, p)
    if lmbda is None:
        R.x, R.y = 0, 0
        return

    x3 = mod_sub(mod_sub(mod_mul(lmbda, lmbda, p), P.x, p), Q.x, p)
    y3 = mod_sub(mod_mul(lmbda, mod_sub(P.x, x3, p), p), P.y, p)
    R.x, R.y = x3, y3
```

MATLAB
```matlab
function R = elptic_sum_finish(R, P, Q, E)
    p = mget();
    lmbda = elliptic_lambda(P, Q, E);
    if isempty(lmbda)
        R.x = 0;
        R.y = 0;
        return;
    end

    x3 = msub(msub(mmul(lmbda, lmbda), P.x), Q.x);
    y3 = msub(mmul(lmbda, msub(P.x, x3)), P.y);
    R.x = x3;
    R.y = y3;
end
```

Grouped view (optional)

Listings 3.4 through 3.6 together implement point addition over a prime field.
