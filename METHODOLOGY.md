# Methodology

## Problem formulation

The project models a two-direction intersection with north-south and east-west traffic. The controller allocates green time to the active direction in response to estimated traffic state. The principal evaluation metric is average vehicle delay.

## v1 controller

The original implementation uses a Mamdani fuzzy controller. Traffic-state inputs are mapped to fuzzy membership values, a rule base determines the output fuzzy set, and defuzzification produces a green-time decision. A genetic algorithm tunes three membership-function centres. The simulator compares fixed-time, default fuzzy and GA-tuned fuzzy control.

## v2 controller

The expanded controller adds the opposing-direction queue as an input alongside the current queue and waiting-time state. It uses a 27-rule fuzzy rule base and a parameterised set of membership breakpoints. The simulator clamps the resulting green duration to its configured physical range.

## Optimisation protocol

GA, PSO and SA are compared using the same fitness definition and evaluation budget. The experiment log describes three random seeds per optimiser/structure combination. The selected candidate is chosen on validation performance, not on the final test set. The final official candidate is `fuzzy2_bp_PSO`.

## Baselines and evaluation

The evaluation compares fixed-time controllers, an actuated queue-based rule, pressure-split and longest-queue-first rules, the original v1 fuzzy controller, and multiple v2 candidates. Scenario demand profiles are synthetically generated and split into training, validation and test sets. Common random numbers are used so each controller sees the same arrivals for a paired comparison. Confidence intervals are estimated by bootstrapping across test scenarios.

## Interpretation

The reported 58.5% reduction is relative to fixed-30 in this simulator. The more demanding comparison is the actuated baseline: v2's measured mean reduction is 4.3%, with a reported 95% bootstrap interval of 2.4% to 6.5%. This should be described as a simulation result rather than evidence of real-world traffic improvement.
