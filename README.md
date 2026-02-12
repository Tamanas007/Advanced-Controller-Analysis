

<div align="center">
🚁 Adaptive Koopman-Assisted LQI Control
Robust • Energy-Aware • Disturbance-Resilient Quadcopter Tracking
</div>
<p align="center"> An adaptive control architecture combining time-varying LQI and online Koopman-style learning using Recursive Least Squares. </p>
📌 Project Summary

This project evaluates and compares three quadcopter control strategies under increasing perturbations and severe unmodeled wind disturbances.

Controller	Adaptive	Energy-Aware	Wind Robust
LQI-Z	❌	❌	⚠️ Limited
KP+LQI	✅	❌	✅
Energy-Aware KP+LQI	✅	✅	✅

The adaptive controller achieves significantly improved tracking accuracy without increasing energy consumption.

🧠 Architecture Overview
1️⃣ LQI-Z (Baseline)

A time-varying Linear Quadratic Integral controller with:

13D quadcopter state

Z-axis integral augmentation

14D augmented system

DARE solved every 10 ms

Anti-windup clamp

Role

Provides a high-precision but fixed-gain baseline.

2️⃣ Adaptive Koopman-Assisted LQI (KP+LQI)
Control Law

u_final = u_lqi + κ · du_assist

Where:

u_lqi → baseline LQI command

du_assist → adaptive correction

κ → dynamic trust factor

🔄 Online Learning

Learns the mapping:

dx_observed = W · du_applied

W ∈ ℝ^(13×4)

Updated continuously via RLS

Captures real system dynamics in real time

🛡 Predictive Acceptance Mechanism

At each timestep:

Predict baseline next state

Predict assisted next state

Compare RMSE (and energy-weighted cost)

Apply assist only if beneficial

This prevents instability under wind and avoids adaptive corruption.

⚡ Energy-Aware Extension

Two additional mechanisms:

Dynamic LQI Scaling

R_eff = R_base (1 + BETA · κ)

Encourages control budget reallocation as trust increases.

Energy-Regularized Assist

High-energy adaptive corrections are penalized directly in the inverse problem.

📊 Performance Highlights
📈 Average Across Perturbations

57% reduction in X RMSE

45% reduction in Z RMSE

Identical average energy consumption

🌪 4N Wind Gust Scenario

Compared to fixed-gain LQI-Z:

67% reduction in X RMSE

25% reduction in Y RMSE

71% reduction in Z RMSE

No increase in energy usage

The controller works smarter, not harder.

🔬 Key Contributions

Online Koopman-style adaptive control via RLS

Predictive assist validation mechanism

Energy-aware control allocation

Robust disturbance rejection

Mission-tunable performance tradeoffs

🏁 Conclusion

The Adaptive KP+LQI architecture provides:

✔ Improved tracking accuracy
✔ Wind robustness
✔ Energy efficiency
✔ Real-world deployability

It represents a practical pathway toward safe integration of online learning within closed-loop control systems.
