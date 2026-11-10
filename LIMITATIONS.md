# Limitations and responsible interpretation

1. **Synthetic demand:** traffic arrival profiles are generated programmatically; no live traffic dataset or field sensor calibration is included in the supplied experiment.
2. **Simplified geometry:** the model is a two-direction intersection abstraction and does not model lane-level turning movements, pedestrians, cyclists or a full road network.
3. **Simplified vehicle dynamics:** queue service and saturation are represented by model assumptions. Real driver behaviour, reaction time, vehicle mix and spillback are not fully represented.
4. **No field validation:** the 3D replay visualises simulated state. It is not a validated digital twin and should not be presented as a deployed traffic-management system.
5. **Baseline sensitivity:** conclusions depend on the chosen baseline. The actuated rule performs much better than fixed-time control, and the v2 fuzzy controller's advantage over it is 4.3%, not 58.5%.
6. **Optimisation variance:** GA, PSO and SA results are close in some configurations; the progress log notes that some optimiser rankings are not decisive relative to run-to-run variation.
7. **Monotonicity:** some trained parameter sets have small measured monotonicity violations. Counts and tolerances are documented in `v2/PROGRESS.md`.
8. **Generalisation:** results apply to the generated held-out scenario set and simulation assumptions. They do not establish performance on arbitrary real intersections.

## Recommended wording

Use: “In the project's held-out simulation scenarios, the validation-selected v2 fuzzy controller reduced mean delay by 58.5% versus fixed-30 and 4.3% versus the actuated baseline.”

Avoid: “The system reduces real-world traffic waiting time by 58.5%.”
