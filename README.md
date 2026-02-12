# 🚁 Adaptive Koopman-Assisted LQI Control

## Robust and Energy-Aware Quadcopter Tracking Under Disturbances

---

## Overview

This project presents a comparative analysis of advanced quadcopter control architectures designed to improve tracking accuracy, robustness, and energy efficiency under increasing perturbations and unmodeled disturbances, including severe wind conditions.

The following controllers are evaluated:

- **LQI-Z** – Linear Quadratic Integral with Z-axis integration  
- **Adaptive KP+LQI** – Koopman-assisted LQI with online RLS learning  
- **Energy-Aware KP+LQI** – Explicit energy-optimized extension  

---

# Controller Architectures

---

## 1. LQI-Z (Baseline Controller)

### Concept

The LQI-Z controller modifies a standard Linear Quadratic Integral controller by integrating only the Z-axis position error. This avoids instability caused by full 3-axis integral augmentation under large perturbations.

### Architecture

- 13-dimensional quadcopter state  
- Single Z-axis integrator (14D augmented system)  
- Time-varying LQI gain recomputed every 10 ms  
- DARE solved at each timestep  
- Anti-windup clamp applied to integrator  

### Characteristics

#### Strengths

- High-precision baseline tracking  
- Strong nominal performance  

#### Limitations

- Fixed-gain behavior  
- Computationally expensive  
- Vulnerable to large unmodeled disturbances  

---

## 2. Adaptive Koopman-Assisted LQI (KP+LQI)

### Control Law

u_final = u_lqi + κ · du_assist  

Where:

- u_lqi = baseline LQI command  
- du_assist = adaptive correction  
- κ = dynamic trust factor  

---

### Online Learning Mechanism

The adaptive learner estimates:

dx_observed = W · du_applied  

Where:

- W ∈ ℝ^(13×4)  
- Updated online using Recursive Least Squares (RLS)  
- Learns system error dynamics in real time  

---

### Predictive Acceptance Mechanism (Core Innovation)

At every timestep:

1. Predict next state using baseline control  
2. Predict next state using assisted control  
3. Compare predicted RMSE and energy-weighted cost  
4. Apply assist only if beneficial  

This prevents instability under disturbances and protects against adaptive corruption.

---

## 3. Energy-Aware Extension

### Energy-Aware LQI Scaling

R_eff = R_base (1 + BETA · κ)

As trust increases, baseline LQI becomes less aggressive, encouraging efficient control allocation.

### Energy-Regularized Assist

High-energy assistive corrections are penalized in the adaptive inverse problem, suppressing inefficient control actions.

---

# Performance Summary

## Average Performance (Across Perturbations)

- 57% reduction in X-axis RMSE  
- 45% reduction in Z-axis RMSE  
- Identical average energy consumption  

---

## High-Disturbance Scenario (4N Wind Gust)

Compared to fixed-gain LQI-Z:

- 67% reduction in X RMSE  
- 25% reduction in Y RMSE  
- 71% reduction in Z RMSE  
- No increase in energy usage  

---

# Key Contributions

- Online Koopman-style adaptive control using RLS  
- Predictive assist validation mechanism  
- Energy-aware cost allocation  
- Robust rejection of unmodeled disturbances  
- Tunable trade-off between tracking accuracy and energy efficiency  

---

# Conclusion

The Adaptive KP+LQI architecture with predictive cost evaluation provides:

- Improved tracking accuracy  
- Increased robustness  
- Energy efficiency  
- Mission-specific tunability  

It represents a practical and deployable approach for integrating online learning into closed-loop quadrotor control systems.
