# Problem 1
 
# Measuring Earth's Gravitational Acceleration with a Pendulum
 
## Introduction
 
This experiment determines the acceleration due to gravity ($g$) using a simple pendulum. The relationship between a pendulum's period ($T$) and length ($L$) is given by:
 
$$
T = 2\pi\sqrt{\frac{L}{g}}
$$
 
Rearranging gives our working formula:
 
$$
g = \frac{4\pi^2 L}{T^2}
$$
 
## Materials and Setup
 
- **Pendulum**: 1.2 m string with 100 g weight  
- **Measuring tools**:
  - Ruler (1 mm resolution, $\Delta L = \pm 0.0005$ m)
  - Stopwatch (0.01 s resolution)  
- **Setup**:  
  $\theta_{max} < 15^\circ$ (small angle approximation)  
  $L$ measured from pivot to center of mass
 
## Data Collection
 
### Length Measurement
 
- $L = 1.200 \pm 0.0005$ m
 
### Time Measurements for 10 Oscillations ($T_{10}$)
 
| Trial | $T_{10}$ (s) | Deviation from Mean (s) |
|-------|--------------|-------------------------|
| 1     | 22.05        | -0.007                  |
| 2     | 22.12        | +0.063                  |
| 3     | 21.98        | -0.077                  |
| 4     | 22.07        | +0.013                  |
| 5     | 22.03        | -0.027                  |
| 6     | 22.10        | +0.043                  |
| 7     | 21.95        | -0.107                  |
| 8     | 22.01        | -0.047                  |
| 9     | 22.08        | +0.023                  |
| 10    | 22.04        | -0.017                  |
 
**Calculations**:
 
- Mean period for 10 oscillations:
 
  $\overline{T}_{10} = \frac{\sum T_{10}}{10} = 22.043 \, \text{s}$
 
- Standard deviation:
 
  $$
  \sigma_T = \sqrt{\frac{\sum (T_{10} - \overline{T}_{10})^2}{9}} = 0.057 \text{ s}
  $$
 
- Uncertainty in mean:
 
  $$
  \Delta T_{10} = \frac{\sigma_T}{\sqrt{10}} = 0.018 \text{ s}
  $$
 
## Calculations
 
1. **Single Period**:
 
    $T = \frac{\overline{T}_{10}}{10} = 2.2043 \text{ s}, \quad \Delta T = \frac{\Delta T_{10}}{10} = 0.0018 \text{ s}$
 
 
2. **Gravitational acceleration**:
 
   $$
   g = \frac{4\pi^2 L}{T^2} = \frac{4\pi^2 \times 1.200}{(2.2043)^2} = 9.74 \text{ m/s}^2
   $$
 
3. **Uncertainty propagation**:
 
   $$
   \frac{\Delta g}{g} = \sqrt{\left(\frac{\Delta L}{L}\right)^2 + \left(2\frac{\Delta T}{T}\right)^2} = \sqrt{(0.00042)^2 + (0.00163)^2} = 0.002
   $$
 
   $$
   \Delta g = 9.74 \times 0.002 = 0.02 \text{ m/s}^2
   $$
 
**Final result**:  
$g = 9.74 \pm 0.02 \text{ m/s}^2$  
*(Standard value: $g = 9.81 \text{ m/s}^2$)*
 
## Uncertainty Analysis
 
| Source          | Contribution to $\Delta g$ | Percentage |
|-----------------|---------------------------|------------|
| Length ($\Delta L$) | $4.2 \times 10^{-4}$      | 0.04%      |
| Timing ($\Delta T$) | $1.63 \times 10^{-3}$     | 99.96%     |
 
## Discussion
 
Key observations:
 
1. Timing uncertainty ($\Delta T$) contributed >99% of total error
2. The 0.7% difference from standard value suggests:
   - Possible systematic error in length measurement
   - Small-angle approximation may not be perfect
   - Air resistance effects
 
**Improvement suggestions**:
 
1. Use photogates to reduce $\Delta T$ by 10x
2. Measure $L$ with digital calipers ($\Delta L \approx 0.01$ mm)
3. Video analysis at 240 fps for precise timing
4. Multiple length measurements for better statistics
 
**Convergence test**:
 
$$
\frac{|g_{exp} - g_{std}|}{\Delta g} = \frac{0.07}{0.02} = 3.5\sigma
$$
 
The result differs by 3.5 standard deviations from the expected value, suggesting unaccounted systematic errors.