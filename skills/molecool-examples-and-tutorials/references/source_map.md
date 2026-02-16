# MoleCool source map: Examples and Tutorials

Generated from source roots:
- `MoleCool`
- `MoleCool/Examples`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "def (main|run_example_scripts)|fast|long|--name|--out|--type|--show" MoleCool/run_examples.py`
- `rg -n "calc_rateeqs|calc_OBEs|add_sidebands|steadystate|trajectory" MoleCool/Examples/core/*.py MoleCool/Examples/misc/*.py`
- `rg -n "if __name__ == '__main__'" MoleCool/Examples/core/*.py MoleCool/Examples/misc/*.py`

## Suggested source entry points (function-level)
- `MoleCool/run_examples.py`: script discovery, name resolution, execution loop, and output-file capture.
- `MoleCool/Examples/misc/plot_SimpleTest1_BaF.py`: compact multi-sideband rate-equation baseline.
- `MoleCool/Examples/misc/plot_SimpleTest2Traj_BaF.py`: trajectory and position-dependent force path.
- `MoleCool/Examples/misc/plot_Simple3+1.py`: minimal OBE workflow with direct plotting.
- `MoleCool/Examples/core/optcycl_3+1levels.py`: optical-cycling OBE setup with steady-state handling.
- `MoleCool/Examples/core/cooling_forces.py`: high-dimensional force scan pattern and steady-state tuning.
- `MoleCool/Examples/core/cooling_forces_RaF.py`: species-specific adaptation pattern for force scans.
- `MoleCool/Examples/core/chirp_slowing.py`: chirped-frequency and trajectory-focused workflow.

## Simulation readiness checks
1. Discover runnable scripts
```bash
python -m MoleCool.run_examples -h
```
Checkpoint: help output lists fast and long example names.

2. Fast-suite validation
```bash
python -m MoleCool.run_examples --name fast
```
Checkpoint: summary table reports `Success` for all fast entries.

3. Single-script artifact generation
```bash
python -m MoleCool.run_examples --name plot_SimpleTest1_BaF.py --out _skill_example_plots --type png
```
Checkpoint: one or more PNG files appear in `_skill_example_plots`.

4. Adapted-script validation
- When copying an example into a custom script, preserve `if __name__ == '__main__':` for multiprocessing-safe execution paths.
- Checkpoint: adapted script completes and reports physically sensible outputs (`system.success`, normalized populations, expected force trend).
