# Numerical Integration of the Damped Harmonic Oscillator — PHYS20762 Computational Physics

Implementation and benchmarking of five numerical ODE integration schemes applied to the forced and unforced damped harmonic oscillator. Methods are validated against the analytical solution and compared via step-size convergence analysis. The best-performing method is then used to investigate damping regimes, impulse forcing, and resonance.

---

## Physics Background

The damped harmonic oscillator is governed by:

```
m·ẍ + b·ẋ + k·x = F(t)
```

with system parameters:
- Mass: m = 5.21 kg
- Damping coefficient: b = 0.1 kg/s
- Spring constant: k = 0.6 N/m

The analytical solution for unforced motion (F = 0) is used as the ground truth against which all numerical methods are compared.

---

## Numerical Methods

Five integration schemes are implemented from first principles:

| Method | Description |
|---|---|
| Euler | First-order forward difference |
| Heun (Improved Euler) | Euler with second-order correction via x += v·dt + (a/2)·dt² |
| Euler-Cromer | Semi-implicit Euler — uses updated velocity to compute position, conserving energy between steps |
| Verlet | Second-order Taylor expansion; first step bootstrapped using Euler-Cromer |
| Runge-Kutta 4 | Four intermediate derivative estimates per step; treats position and velocity as independent state variables |

---

## Analysis Pipeline

### 1. Method Validation
Each method is plotted against the analytical solution for the unforced system. Residuals between numerical and analytical displacement are computed at the final time step.

### 2. Step-size Convergence Analysis
Error is computed across eight step sizes (0.001 to 0.2 s) for each method and plotted on a log-log scale. All four non-RK4 methods show approximately linear error scaling with step size. Verlet integration achieves the lowest error intercept and is selected for all subsequent simulations. RK4 scales as O(dt⁴) but is excluded due to computational cost.

### 3. Damping Regimes
The critical damping coefficient b_c = 2√(km) is computed. Simulations are run at b = b_c/2 (underdamped), b = b_c (critical), and b > b_c (overdamped), confirming expected behaviour in each regime.

### 4. Impulse Forcing
Several forcing functions are applied at t = 20 s and their effects compared:
- **Top-hat pulse** (positive and negative direction)
- **Delayed top-hat** (applied after 5 oscillation periods)
- **Constant force** (step function) — reveals that a constant force shifts the equilibrium position
- **Gaussian pulse** — replaces the unphysical discontinuity of the top-hat with a smooth, physically motivated force profile normalised to equal impulse

### 5. Sinusoidal Forcing and Resonance
A sinusoidal external force F(t) = A·sin(ωt) is applied across a range of driving frequencies. Amplitude is recorded after the initial transient decays. Resonance curves are plotted for multiple damping values, confirming the expected increase in peak amplitude and sharpening of the resonance peak as damping decreases.

### 6. Animations
Matplotlib animations of the oscillator trajectory are produced for selected simulations using Verlet integration.

---

## Requirements

```
numpy
matplotlib
scipy
```

Install with:
```bash
pip install numpy matplotlib scipy
```

---

## Usage

Open `project2-complete.ipynb` in Jupyter and run cells sequentially. No external data files are required. Note that animation cells take additional time to render and may slow notebook loading — these can be skipped without affecting the rest of the analysis.
