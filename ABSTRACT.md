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
closed-loop performance and useful planning behavior from the learned world
model. In the latest private verdict, the planning controller passed current
sweep/coupling gates while the amortized real-time actor narrowly failed the
sweep gate. A gray-box residual head improved over a pure neural decoder and
beat the physics baseline at one-step EDGE prediction, but it remained worse
than physics over full open-loop EDGE rollouts. Remaining limitations include
residual drift, response-magnitude calibration, incomplete lateral
observability, simulator fidelity, and the absence of flight-test validation.

The near-term objective is to convert short-horizon learned corrections into
stable long-horizon behavior, distill planner performance into the real-time
actor, and then run end-to-end runtime assurance and fault-injection tests.
