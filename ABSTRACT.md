# Abstract

This project investigates a safety-supervised, model-based reinforcement
learning architecture for coupled high-speed flight control. A unified latent
world model learns transitions over longitudinal, lateral, propulsion, thermal,
and atmospheric variables while an amortized actor produces real-time control
commands. A planning controller provides a decision-time comparison, and
conventional controllers remain available through a supervisory fallback
stack.

Evaluation separates closed-loop reward from physical understanding. The
learned model is probed using control-surface response sweeps, cross-axis
coupling tests, and short-horizon prediction maps over Mach-altitude cells.
Physical feasibility is enforced using dynamic-pressure and thermal-envelope
constraints so that extrapolation outside the safe envelope is not confused
with nominal flight capability. Interim simulation results show positive
closed-loop performance, longitudinal response across subsonic-to-supersonic
conditions, measurable supersonic lateral coupling, and low aggregate
short-horizon error over most feasible cells. Remaining limitations include
response-magnitude calibration, incomplete lateral observability, simulator
fidelity, and the absence of flight-test validation.

The near-term objective is to determine whether error-driven curriculum
learning improves weak feasible regions without degrading broad-envelope
control, followed by end-to-end runtime assurance and fault-injection tests.

