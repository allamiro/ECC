Point Representation and Utilities

Listing 3.1: Point and Curve Structures

Python
```python
from dataclasses import dataclass


@dataclass
class Point:
    x: int
    y: int


@dataclass
class Curve:
    a4: int
    a6: int
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

Listing 3.2: Point and Curve Initialization

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

Grouped view (optional)

Listings 3.1 through 3.3 define point/curve containers and basic utilities.
