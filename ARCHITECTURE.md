# Architecture and execution flow

## High-level flow

1. **Scenario generation (`v2/scenarios.py`)** creates synthetic demand profiles and stores fixed-seed TRAIN, VALIDATION, TEST and legacy scenario sets in `v2/data/scenarios.json`.
2. **Traffic simulation (`v2/simulate2.py`)** generates/uses reproducible arrivals, tracks queues and delay in both directions, and can emit per-second trace data.
3. **Controllers (`v2/controllers.py`, `v2/fuzzy2.py`)** choose the green duration from queue, waiting-time and opposing-queue inputs. Baselines include fixed-time and queue-responsive policies.
4. **Optimisation (`v2/optimizers.py`, `v2/run_optimizers.py`)** tunes fuzzy-controller parameters using GA, PSO and SA under a shared evaluation budget.
5. **Selection and evaluation (`v2/evaluate_final.py`)** selects the candidate using validation data, then evaluates the chosen candidate on held-out test scenarios with paired comparisons and bootstrap confidence intervals.
6. **Explainability and demo (`v2/predict.py`, `v2/server.py`)** expose a single controller decision and a browser-based simulation/replay.

## v1 and v2

The top-level Python files implement the original v1 PBL pipeline. The expanded v2 implementation is isolated under `v2/`; it retains a v1 controller adapter for controlled comparison. The original files and review artefacts are kept rather than silently replacing the earlier implementation.

## Prototype request flow

The browser requests controller metadata and simulation results from the local Python HTTP server. The server validates the selected controller and inputs, invokes the Python simulation, and returns trace/result data used by the 3D viewer. The visualisation uses vendored Three.js assets rather than remote script dependencies.

## Reproducibility boundaries

- Scenario generator seeds and simulation seeds are stored with the scenario data.
- All controllers in a paired comparison receive identical pre-drawn arrivals.
- The test set is not used for optimiser selection; the official evaluation is guarded by a marker file.
- `v2/PROGRESS.md` records decisions, discrepancies and known caveats.
