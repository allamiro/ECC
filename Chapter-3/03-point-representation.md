Point Representation and Utilities

Listing 3.1: Point and Curve Structures

We keep points and curves as small containers so code stays readable. In Python we use
simple classes (no dataclass required), and in MATLAB we use structs.

Python
```python
class Point:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y


class Curve:
    def __init__(self, a4=0, a6=0):
        self.a4 = a4
        self.a6 = a6
```

MATLAB
```matlab
function P = point_make(x, y)
    P.x = x;
    P.y = y;
end

function E = curve_make(a4, a6)
    E.a4 = a4;
    E.a6 = a6;
end
```

Example usage

Python
```python
P = Point(3, 10)
E = Curve(a4=23, a6=-1)
```

MATLAB
```matlab
P = point_make(3, 10);
E = curve_make(23, -1);
```

Listing 3.2: Point and Curve Initialization

These helpers mirror the C init/clear routines. In Python and MATLAB this is mostly
convenience, but it keeps the API consistent.

Python
```python
def point_init():
    return Point(0, 0)


def point_clear(_P):
    # No-op in Python
    return


def curve_init(a4=0, a6=0):
    return Curve(a4, a6)


def curve_clear(_E):
    # No-op in Python
    return
```

MATLAB
```matlab
function P = point_init()
    P.x = 0;
    P.y = 0;
end

function point_clear(~)
    % No-op in MATLAB for structs
end

function E = curve_init(a4, a6)
    E.a4 = a4;
    E.a6 = a6;
end

function curve_clear(~)
    % No-op in MATLAB for structs
end
```

Listing 3.3: Point Copy and Test

We often need to copy points and check if a point is the point at infinity (0, 0).

Python
```python
def point_copy(P):
    return Point(P.x, P.y)


def test_point(P):
    return P.x == 0 and P.y == 0
```

MATLAB
```matlab
function R = point_copy(P)
    R.x = P.x;
    R.y = P.y;
end

function tf = test_point(P)
    tf = (P.x == 0) && (P.y == 0);
end
```

Plot: Finite Field Points (Example p = 43)

Dependencies
- Python: `matplotlib`

Python
```python
import matplotlib.pyplot as plt


def plot_field_points():
    # Example curve: y^2 = x^3 + 23x - 1 mod 43
    p = 43
    a4, a6 = 23, -1
    xs, ys = [], []
    for x in range(p):
        rhs = (x**3 + a4 * x + a6) % p
        for y in range(p):
            if (y * y) % p == rhs:
                xs.append(x)
                ys.append(y)
    plt.scatter(xs, ys, s=12)
    plt.title("Finite Field Points: y^2 = x^3 + 23x - 1 mod 43")
    plt.xlabel("x")
    plt.ylabel("y")
    plt.grid(True)
    plt.show()


if __name__ == "__main__":
    plot_field_points()
```

MATLAB
```matlab
function plot_field_points()
    % Example curve: y^2 = x^3 + 23x - 1 mod 43
    p = 43;
    a4 = 23;
    a6 = -1;
    xs = [];
    ys = [];
    for x = 0:p-1
        rhs = mod(x^3 + a4 * x + a6, p);
        for y = 0:p-1
            if mod(y^2, p) == rhs
                xs(end+1) = x; %#ok<AGROW>
                ys(end+1) = y; %#ok<AGROW>
            end
        end
    end
    scatter(xs, ys, 12, 'filled');
    title('Finite Field Points: y^2 = x^3 + 23x - 1 mod 43');
    xlabel('x');
    ylabel('y');
    grid on;
end
```

How to run the plot
- Python: save as `plot_field_points.py` and run `python plot_field_points.py`
- MATLAB: save as `plot_field_points.m` and run `plot_field_points`

Expected result
- A scatter plot of all finite-field points on the example curve.

Grouped view (optional)

Listings 3.1 through 3.3 define point/curve containers and basic utilities.
