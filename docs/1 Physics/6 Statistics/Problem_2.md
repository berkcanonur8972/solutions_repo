Below is a comprehensive Markdown document that addresses the task of estimating π using Monte Carlo simulations for both the circle-based method and Buffon’s Needle problem. The document includes theoretical explanations, Python code for simulations, visualizations, and analysis of convergence. The code is designed to run in Google Colab, utilizing NumPy for random number generation and Matplotlib for plotting. Mathematical expressions are formatted using `$$` for LaTeX rendering in Markdown.


# Estimating π Using Monte Carlo Simulations

Monte Carlo methods leverage randomness to estimate numerical values, such as the mathematical constant π. This document explores two elegant approaches: the circle-based method, which uses geometric probability in a 2D plane, and Buffon’s Needle problem, which estimates π through the probability of a needle crossing parallel lines. Both methods are implemented in Python, visualized, and analyzed for convergence and efficiency.

## Part 1: Estimating π Using a Circle

### 1.1 Theoretical Foundation

Consider a unit circle (radius \( r = 1 \)) centered at the origin, inscribed in a square with side length 2 (spanning \( x, y \in [-1, 1] \)). The area of the circle is:

$$ A_{\text{circle}} = \pi r^2 = \pi \cdot 1^2 = \pi $$

The area of the square is:

$$ A_{\text{square}} = 2 \cdot 2 = 4 $$

If we randomly generate points uniformly within the square, the probability that a point lies inside the circle (i.e., \( x^2 + y^2 \leq 1 \)) equals the ratio of the circle’s area to the square’s area:

$$ P(\text{point in circle}) = \frac{A_{\text{circle}}}{A_{\text{square}}} = \frac{\pi}{4} $$

Thus, if we generate \( N \) points and \( M \) fall inside the circle, the ratio \( \frac{M}{N} \) approximates \( \frac{\pi}{4} \), leading to the estimate:

$$ \pi \approx 4 \cdot \frac{M}{N} $$

### 1.2 Simulation

We generate \( N \) random points in the square \( [-1, 1] \times [-1, 1] \), count how many lie inside the unit circle, and compute the estimate for π.

### 1.3 Visualization

The following Python code generates a scatter plot distinguishing points inside and outside the unit circle.

```python
import numpy as np
import matplotlib.pyplot as plt

# Set random seed for reproducibility
np.random.seed(42)

def estimate_pi_circle(N):
    # Generate N random points in [-1, 1] x [-1, 1]
    x = np.random.uniform(-1, 1, N)
    y = np.random.uniform(-1, 1, N)
    
    # Check which points lie inside the unit circle
    inside_circle = x**2 + y**2 <= 1
    M = np.sum(inside_circle)
    
    # Estimate pi
    pi_estimate = 4 * M / N
    
    return x, y, inside_circle, pi_estimate

# Run simulation with N = 10000
N = 10000
x, y, inside_circle, pi_estimate = estimate_pi_circle(N)

# Plot
plt.figure(figsize=(8, 8))
plt.scatter(x[inside_circle], y[inside_circle], c='blue', s=1, label='Inside Circle')
plt.scatter(x[~inside_circle], y[~inside_circle], c='red', s=1, label='Outside Circle')
circle = plt.Circle((0, 0), 1, edgecolor='black', facecolor='none')
plt.gca().add_patch(circle)
plt.gca().set_aspect('equal')
plt.xlim(-1, 1)
plt.ylim(-1, 1)
plt.title(f'Circle Method: π ≈ {pi_estimate:.5f}, N = {N}')
plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.grid(True)
plt.show()

print(f"Estimated π: {pi_estimate}")
```
![alt text](image-3.png)

**Output**: The plot shows blue points inside the unit circle and red points outside, with the estimated π value displayed in the title.

### 1.4 Analysis

To study convergence, we compute the absolute error \( |\pi_{\text{estimate}} - \pi| \) for increasing \( N \).

```python
import numpy as np
import matplotlib.pyplot as plt

# Monte Carlo yöntemiyle pi tahmini fonksiyonu
def estimate_pi_circle(N):
    np.random.seed(0)  # Tekrarlanabilirlik için
    x = np.random.rand(N)
    y = np.random.rand(N)
    inside_circle = (x**2 + y**2) <= 1
    count_inside = np.sum(inside_circle)
    pi_estimate = 4 * count_inside / N
    return x, y, inside_circle, pi_estimate

# Farklı nokta sayılarıyla hata analizi
Ns = np.logspace(2, 6, num=50, dtype=int)  # 100'den 1,000,000'a kadar logaritmik artışla
errors = []
pi_true = np.pi

for N in Ns:
    _, _, _, pi_estimate = estimate_pi_circle(N)
    errors.append(abs(pi_estimate - pi_true))

# Hata grafiği (log-log eksenlerde)
plt.figure(figsize=(10, 6))
plt.loglog(Ns, errors, 'o-', label='Absolute Error')
plt.title('Convergence of Circle-Based π Estimation')
plt.xlabel('Number of Points (N)')
plt.ylabel('Absolute Error |π_estimate - π|')
plt.grid(True, which='both')
plt.legend()
plt.tight_layout()
plt.show()
```
![alt text](image-4.png)

**Observation**: The error decreases roughly as \( O(1/\sqrt{N}) \), typical for Monte Carlo methods, due to the central limit theorem. Computationally, the method is efficient, with \( O(N) \) time complexity for generating and checking points.

## Part 2: Estimating π Using Buffon’s Needle

### 2.1 Theoretical Foundation

Buffon’s Needle problem involves dropping a needle of length \( l \) onto a plane with parallel lines spaced \( d \) units apart (\( l \leq d \)). The probability that the needle crosses a line depends on its random position and orientation.

Consider a needle with midpoint at position \( (x, y) \) and angle \( \theta \) relative to the horizontal. The needle crosses a line if the vertical distance from its midpoint to the nearest line, \( \frac{d}{2} \cdot \sin(\theta) \), is less than half the needle’s projected length. For simplicity, assume \( l = d \). The probability of a crossing is derived from geometric probability:

$$ P(\text{crossing}) = \frac{2l}{\pi d} $$

For \( l = d \):

$$ P(\text{crossing}) = \frac{2}{\pi} $$

If we drop \( N \) needles and \( M \) cross a line, the ratio \( \frac{M}{N} \approx \frac{2}{\pi} \), so:

$$ \pi \approx \frac{2N}{M} $$

### 2.2 Simulation

We simulate dropping needles, assuming \( l = d = 1 \), and count line crossings.

### 2.3 Visualization

The following code visualizes needle positions relative to parallel lines.

```python
import numpy as np
import matplotlib.pyplot as plt

# Set random seed
np.random.seed(42)

def estimate_pi_buffon(N, l=1, d=1):
    # Random midpoint x (y is irrelevant for horizontal lines), and angle theta
    x_mid = np.random.uniform(0, d, N)  # Midpoint x between 0 and d
    theta = np.random.uniform(0, np.pi/2, N)  # Angle between 0 and π/2
    
    # Distance from midpoint to nearest line
    crossings = np.abs(l/2 * np.sin(theta)) >= (x_mid % d)
    M = np.sum(crossings)
    
    # Estimate pi
    pi_estimate = 2 * N / M if M > 0 else np.inf
    
    return x_mid, theta, crossings, pi_estimate

# Run simulation with N = 1000
N = 1000
x_mid, theta, crossings, pi_estimate = estimate_pi_buffon(N)

# Plot
plt.figure(figsize=(10, 4))
for i in range(min(N, 50)):  # Plot up to 50 needles
    x_c = x_mid[i]
    t = theta[i]
    x1 = x_c - 0.5 * np.cos(t)
    x2 = x_c + 0.5 * np.cos(t)
    y1 = -0.5 * np.sin(t)
    y2 = 0.5 * np.sin(t)
    color = 'blue' if crossings[i] else 'red'
    plt.plot([x1, x2], [y1, y2], color, alpha=0.5)
    
# Plot lines
for x in [0, 1]:
    plt.axvline(x, color='black', linestyle='--')
    
plt.title(f'Buffon’s Needle: π ≈ {pi_estimate:.5f}, N = {N}')
plt.xlabel('x')
plt.ylabel('y')
plt.gca().set_aspect('equal')
plt.grid(True)
plt.show()

print(f"Estimated π: {pi_estimate}")
```
![alt text](image-5.png)


**Output**: Blue needles cross the lines, red ones do not. The estimated π is shown in the title.

### 2.4 Analysis

We analyze convergence similarly to the circle method.

```python
# Convergence analysis
errors_buffon = []
for N in Ns:
    _, _, _, pi_estimate = estimate_pi_buffon(N)
    errors_buffon.append(abs(pi_estimate - pi_true))

# Plot convergence
plt.figure(figsize=(10, 6))
plt.loglog(Ns, errors, 'o-', label='Circle Method')
plt.loglog(Ns, errors_buffon, 's-', label='Buffon’s Needle')
plt.title('Convergence Comparison')
plt.xlabel('Number of Iterations (N)')
plt.ylabel('Absolute Error |π_estimate - π|')
plt.grid(True)
plt.legend()
plt.show()
```

![alt text](image-6.png)

## Comparison of Methods

- **Accuracy**: Both methods exhibit \( O(1/\sqrt{N}) \) convergence, but the circle method typically has lower variance due to the higher probability of points falling inside the circle (\( \frac{\pi}{4} \approx 0.785 \)) compared to needle crossings (\( \frac{2}{\pi} \approx 0.637 \)).
- **Computational Efficiency**: The circle method involves simpler computations (checking \( x^2 + y^2 \leq 1 \)) versus Buffon’s Needle (trigonometric functions and modulo operations). The circle method is generally faster.
- **Intuitiveness**: The circle method is more visually intuitive, directly relating to the geometry of π, while Buffon’s Needle is less obvious but historically significant.

## Conclusion

Both Monte Carlo methods effectively estimate π, with the circle-based approach being more efficient and accurate for the same number of iterations. Visualizations enhance understanding, and convergence analysis confirms the expected \( O(1/\sqrt{N}) \) behavior. These simulations highlight the power of randomness in numerical computation.



### Notes for Running in Colab

1. **Dependencies**: The code uses `numpy` and `matplotlib`, which are pre-installed in Google Colab. No additional setup is required.
2. **Execution**: Copy each code block into a Colab cell and run. The plots will display inline.
3. **Scalability**: For large \( N \), the visualization may become cluttered. The code limits needle plotting to 50 for clarity.
4. **Random Seed**: Set for reproducibility, but you can remove `np.random.seed(42)` for varied results.

This response provides a complete solution with theoretical derivations, working code, visualizations, and comparative analysis, all formatted for Markdown rendering in Colab.