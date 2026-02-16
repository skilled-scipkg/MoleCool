# MoleCool source map: Getting Started

Generated from source roots:
- `MoleCool`
- `MoleCool/constants`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "def (__init__|calc_rateeqs|calc_OBEs|check_config|_verify_calculation|initialize_N0)" MoleCool/System.py`
- `rg -n "def (add_all_levels|add_electronicstate|load_states|check_config)" MoleCool/Levelsystem.py`
- `rg -n "def (add|add_sidebands|check_config)" MoleCool/Lasersystem.py`
- `rg -n "def (turnon|get_remix_matrix|reset)" MoleCool/Bfield.py`
- `rg -n "def (main|run_example_scripts)" MoleCool/run_examples.py`

## Suggested source entry points (function-level)
- `MoleCool/__init__.py`: first import surface for `System`, `Lasersystem`, `Levelsystem`, and `Bfield`.
- `MoleCool/System.py`: `System.__init__`, `System.calc_rateeqs`, `System.calc_OBEs`, `System.check_config`, `System._verify_calculation`.
- `MoleCool/Levelsystem.py`: `Levelsystem.add_all_levels`, `Levelsystem.add_electronicstate`, `ElectronicState.load_states`, `Levelsystem.check_config`.
- `MoleCool/Lasersystem.py`: `Lasersystem.add`, `Lasersystem.add_sidebands`, `Lasersystem.check_config`.
- `MoleCool/Bfield.py`: `Bfield.turnon`, `Bfield.get_remix_matrix`, `Bfield.reset`.
- `MoleCool/run_examples.py`: startup smoke-test path and example CLI behavior.
- `MoleCool/constants/138BaF.json`: default beginner constants used by docs and examples.

## Simulation readiness checks
1. Baseline package smoke test
```bash
python -m MoleCool.run_examples --name fast
```
Checkpoint: all listed scripts report `Success`.

2. First custom rate-equation run
```bash
python - <<'PY'
from MoleCool import System

system = System(load_constants='138BaF', verbose=False)
system.levels.add_all_levels(v_max=0)
system.lasers.add_sidebands(
    lamb=859.83e-9,
    P=20e-3,
    sidebands=[-2, -1, 1, 2],
    ratios=[0.8, 1.0, 1.0, 0.8],
    mod_freq=39.33e6,
    offset_freq=19e6,
)
system.Bfield.turnon(strength=5e-4, direction=[1, 1, 1])
system.calc_rateeqs(t_int=8e-6, magn_remixing=True, verbose=False)

print("success:", system.success)
print("message:", system.message)
print("population_sum:", float(system.N[:, -1].sum()))
PY
```
Checkpoint: `success` is `True` and `population_sum` stays close to 1.

3. Trajectory-path spot check
```bash
python - <<'PY'
from MoleCool import System

system = System(load_constants='138BaF', verbose=False)
system.levels.add_all_levels(v_max=0)
system.lasers.add(lamb=860e-9, P=20e-3, pol='lin')
system.calc_rateeqs(
    t_int=2e-6,
    trajectory=True,
    position_dep=True,
    verbose=False,
)

print("success:", system.success)
print("v_shape:", system.v.shape)
print("r_shape:", system.r.shape)
PY
```
Checkpoint: `success` is `True` and both `v_shape` and `r_shape` are `(3, Nt)`.
