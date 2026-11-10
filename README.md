Adaptive Traffic Signal Optimization

[LIVE INTERACTIVE 3D DEMO — CLICK HERE](https://madhang12.github.io/adaptive-traffic-signal-optimization/replay_3d.html)

[View 2D Simulation](https://madhang12.github.io/adaptive-traffic-signal-optimization/replay.html)

**A team-built traffic-intersection simulation and adaptive signal-control project using fuzzy logic, genetic algorithms, particle swarm optimisation and simulated annealing.**

This repository contains two documented generations of the project:

- **v1 — original PBL implementation:** a Mamdani fuzzy controller whose membership-function parameters are tuned with a genetic algorithm.
- **v2 — expanded research/prototype implementation:** a three-input fuzzy controller, reproducible traffic scenarios, GA/PSO/SA optimisation, stronger baselines, statistical evaluation and an interactive 3D replay prototype.

The v2 implementation is the recommended starting point for understanding the latest experiments. The original v1 implementation and its supporting review material are retained for traceability.

## Key results (v2 held-out evaluation)

The official v2 evaluation uses **60 held-out traffic scenarios**, each evaluated with three traffic seeds. The validation-selected `fuzzy2_bp_PSO` controller achieved:

| Comparison | Mean delay reduction | 95% bootstrap CI |
|---|---:|---:|
| Fixed 30-second timer | **58.5%** | 51.7% to 65.1% |
| Best-fixed 29-second baseline | **58.6%** | 51.8% to 65.1% |
| Original v1 fuzzy + GA | **42.4%** | 36.2% to 48.5% |
| Actuated queue-based baseline | **4.3%** | 2.4% to 6.5% |

These are simulation results, not field-deployment results. The simpler actuated baseline is strong; the v2 fuzzy controller's measured advantage over it is comparatively modest. See [`v2/RESULTS.md`](v2/RESULTS.md) for the full tables, confidence intervals, per-profile results and optimizer comparison.

### Important clarification about the earlier 27.8% claim

The original v1 README's **27.8%** headline is not the final verified figure. The v1 five-scenario legacy re-evaluation reports **26.8%** mean reduction against fixed-30 (single seed); the two legacy held-out scenarios average **37.9%**. These v1 numbers are separate from the larger v2 held-out evaluation. Do not mix the figures or present 27.8% as the final result.

## Interactive prototype

Run the local Python server from the repository root:

```bash
python -m pip install -r v2/requirements.txt
python v2/server.py
```

Then open <http://localhost:8000>. The browser interface sends controller inputs to the Python simulator and displays a 3D replay. Three.js and OrbitControls are included locally under `vendor/`, so the visualisation does not need a CDN connection.

Useful API routes include:

- `GET /healthz` — server health
- `GET /api/controllers` — available controller configurations
- `GET /api/predict?queue=30&wait=60&opp=8` — explainable fuzzy decision
- `POST /api/simulate` — run a simulation (JSON body, e.g. `{"preset":"uneven","seed":99,"controller":"actuated"}`)

## Run tests

```bash
python -m unittest discover -s v2/tests -v
```

The test suite covers membership functions, rule completeness, deterministic simulation, v1 compatibility, scenario generation, optimiser behaviour, explainability, API validation and simulation invariants.

## Reproduce experiments

```bash
python v2/run_all.py --quick
```

The quick mode reuses the saved optimiser runs. A full optimiser rerun is substantially more expensive:

```bash
python v2/run_all.py
```

The final TEST set is deliberately treated as a held-out evaluation set. `v2/data/TEST_TOUCHED` records that the official test has been run; the evaluation script protects against accidentally reusing it for model selection. Read `v2/README.md` and `v2/PROGRESS.md` before attempting a full rerun.

## Project structure

```text
.
├── fuzzy_core.py, simulate.py, ga_tune.py, final_comparison.py   # v1 implementation
├── results*.json, trace_*.json, replay*.html                    # v1 outputs/replays
├── v2/
│   ├── fuzzy2.py, controllers.py, simulate2.py                  # v2 controller/simulator
│   ├── scenarios.py, optimizers.py, run_optimizers.py            # scenario generation + GA/PSO/SA
│   ├── evaluate_final.py, results_v2.json, RESULTS.md            # held-out evaluation
│   ├── predict.py, server.py, replay_3d_v2.html                  # explainability + prototype
│   ├── tests/                                                    # automated tests
│   └── data/                                                     # frozen scenarios and controller artifacts
├── docs/                                                         # original PBL report, deck and script
└── vendor/                                                       # local Three.js dependencies
```

## Algorithms and methods

- Mamdani fuzzy inference with membership functions and a 27-rule v2 rule base.
- Genetic Algorithm (GA), Particle Swarm Optimisation (PSO) and Simulated Annealing (SA).
- Fixed-time, actuated, pressure-split and longest-queue-first comparison controllers.
- Paired simulation using common pre-drawn arrivals, so controllers face the same traffic inputs.
- Held-out test scenarios and bootstrap confidence intervals over scenarios.

See [`v2/FINAL_REPORT.md`](v2/FINAL_REPORT.md), [`v2/DECK_NUMBERS.md`](v2/DECK_NUMBERS.md), [`v2/RESULTS.md`](v2/RESULTS.md) and [`docs/`](docs/) for detail.

## Assumptions and limitations

- This is a simulated two-direction intersection, not a deployment-ready traffic controller.
- Arrival rates are generated from synthetic, piecewise-linear demand profiles; they are not measured from live roads.
- Vehicle movement, saturation/service rates and signal switching are simplified model assumptions.
- The model does not represent full road geometry, pedestrian phases, turning movements, emergency-vehicle priority, weather, incidents or a network of connected intersections.
- The 3D replay is a visualisation of the simulation, not a calibrated digital twin.
- Trained fuzzy controllers are not guaranteed to be perfectly monotone for every possible input; measured monotonicity checks are documented in the progress/results material.
- Reported improvements are conditional on the simulator, selected scenarios, controller definitions and evaluation protocol.

## Team and academic context

This was developed as a team academic project. The PBL report credits **G. Madhan, D. Ritheek, M. Srilakshmi Reddy, B. Deekshan Raj, P. Priyakaran Reddy and **. The original report, review deck and presentation script are retained in `docs/`.

This repository is published as a portfolio record of participation in the team project. It does not imply that the project was independently authored by one person. Individual contributions should be described only where they can be substantiated.

## Acknowledgement and provenance

The original project materials and team report are retained in this repository. The v2 experiment log documents the expanded evaluation and implementation decisions. Please consult the report and progress log when citing results or describing the project.
