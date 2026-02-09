Elliptic Curve Algebra

Elliptic curves over the reals let us visualize the same formulas used over finite
fields. Given two points \(P\) and \(Q\) on the curve, the line through them intersects
the curve at a third point, and the sum is the reflection across the x-axis.

Point at infinity:
- The identity element is the point at infinity, denoted \(0\)
- For ordinary curves in this book it is represented as \((0, 0)\) in code

For the ordinary curve:

$$
y^2 = x^3 + a_4x + a_6
$$

the negative of a point is:

$$
-(x, y) = (x, -y)
$$

Slope for addition (works for distinct points and doubling):

$$
\lambda = \frac{x_1^2 + x_1x_2 + x_2^2 + a_4}{y_1 + y_2}
$$

Resulting point \(R = P_1 + P_2 = (x_3, y_3)\):

$$
x_3 = \lambda^2 - x_1 - x_2
$$

$$
y_3 = \lambda(x_1 - x_3) - y_1
$$

Plot: Elliptic Curve Over the Reals

Python
```python
import numpy as np
import matplotlib.pyplot as plt


def plot_real_curve():
    # Example curve: y^2 = x^3 - 5x + 5
    xs = np.linspace(-4, 4, 800)
    rhs = xs**3 - 5 * xs + 5
    mask = rhs >= 0
    ys = np.sqrt(rhs[mask])
    plt.plot(xs[mask], ys, "b")
    plt.plot(xs[mask], -ys, "b")
    plt.title("Elliptic Curve: y^2 = x^3 - 5x + 5")
    plt.xlabel("x")
    plt.ylabel("y")
    plt.grid(True)
    plt.show()


if __name__ == "__main__":
    plot_real_curve()
```

MATLAB
```matlab
function plot_real_curve()
    % Example curve: y^2 = x^3 - 5x + 5
    x = linspace(-4, 4, 800);
    rhs = x.^3 - 5 .* x + 5;
    mask = rhs >= 0;
    y = sqrt(rhs(mask));
    plot(x(mask), y, 'b', x(mask), -y, 'b');
    title('Elliptic Curve: y^2 = x^3 - 5x + 5');
    xlabel('x');
    ylabel('y');
    grid on;
end
```
