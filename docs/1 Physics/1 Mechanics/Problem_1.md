# Problem 1
import numpy as np
import matplotlib.pyplot as plt

# Constants
g = 9.81  # Acceleration due to gravity (m/s^2)

# Function to calculate range
def calculate_range(v0, theta):
    return (v0**2 * np.sin(2 * np.radians(theta))) / g

# Initial conditions
v0 = 20  # Initial velocity (m/s)
angles = np.linspace(0, 90, 100)  # Angles from 0 to 90 degrees

# Calculate ranges
ranges = [calculate_range(v0, angle) for angle in angles]

# Plot the results
plt.figure(figsize=(8, 5))
plt.plot(angles, ranges, label=f'v0 = {v0} m/s')
plt.title('Projectile Range vs Angle of Projection')
plt.xlabel('Angle of Projection (degrees)')
plt.ylabel('Range (meters)')
plt.grid(True)
plt.legend()
plt.show()



