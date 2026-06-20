# June 2026 Research Status

This note summarizes the current public-safe status of the private Hypersonic
Pilot research program. It intentionally omits implementation code, model
weights, raw logs, infrastructure details, and safety configuration.

## Current Claim

The project has demonstrated that a unified learned world model can support
useful planning behavior in a high-speed flight simulation setting. It has not
yet demonstrated that the learned model has discovered a broadly superior
general flight law compared with the hand-coded physics baseline.

That distinction matters. The model is useful; the scientific claim is still
bounded.

## Latest Architecture Under Test

The current private implementation combines:

- a unified latent world model over coupled flight dynamics;
- an amortized actor for real-time control;
- an MPPI-style planner for slower decision-time search;
- conventional fallback controllers;
- a deterministic safety and envelope filter;
- a gray-box residual head that learns corrections on top of a physics
  predictor.

The residual head was recently changed from one-step-only supervision to a
short multi-step rollout objective. This was intended to reduce drift when the
gray-box model is applied repeatedly to its own predicted states.

## Latest Verdict

The latest private simulation run completed a 500k-step unified retrain with
withheld edge cells and multi-step residual training.

Controller verdict:

| Controller | Checkpoint | Mean return | Worst sweep | Coupling | Verdict |
|---|---|---:|---:|---:|---|
| Actor | selected best | +1640.2 | 0.563 | 0.500 | sweep fail, coupling pass |
| Actor | final | +1063.0 | 0.576 | 0.500 | sweep fail, coupling pass |
| MPPI planner | selected best | +1146.6 | 0.699 | 0.500 | pass |
| MPPI planner | final | +1363.6 | 0.686 | 0.500 | pass |

Interpretation: the learned model is presently more reliable when used for
planning than when compressed into the real-time actor.

Generalization-law verdict:

| EDGE excited/full predictor | Selected best | Final checkpoint |
|---|---:|---:|
| Physics baseline | 0.0096 | 0.0096 |
| Neural decoder | 0.0850 | 0.1139 |
| Gray-box residual | 0.0335 | 0.0246 |

The final gray-box residual improved full-horizon EDGE error versus the
selected checkpoint, but it remained worse than the physics baseline.

One-step EDGE prediction was different:

| EDGE excited/h1 predictor | Final checkpoint |
|---|---:|
| Physics baseline | 0.0049 |
| Neural decoder | 0.1092 |
| Gray-box residual | 0.0027 |

Interpretation: the residual head learned useful local correction information,
but that correction is not yet stable over longer open-loop rollouts.

## Why This Is Not Yet A Broad Equation-Discovery Claim

The current model is optimized for control and prediction, not symbolic
equation discovery. It can encode useful hidden structure without exposing a
human-readable law. However, the strongest proof test asks whether the learned
gray-box predictor beats the physics baseline on held-out edge cells over full
rollouts. It does not yet do that.

The correct current statement is:

> The learned world model supports useful planning and contains local residual
> correction signal, but it has not yet surpassed the engineered physics
> baseline as a broad long-horizon dynamics law.

## Near-Term Research Direction

The next phase is focused on:

1. making checkpoint selection robustness-first instead of return-first;
2. distilling the passing MPPI behavior into the real-time actor;
3. improving gray-box rollout stability beyond short horizons;
4. using feasible weak-cell curriculum learning without training on
   structurally invalid regimes;
5. testing the runtime assurance stack under sensor and actuator faults.

## Boundary

No result described here is a claim of real aircraft readiness, hypersonic
vehicle certification, or validated flight-test performance. The current scope
is simulation research inside the credible envelope of the available simulator.
