# MoleCool source map: API and Scripting

Generated from source roots:
- `MoleCool`
- `pyproject.toml`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "from \\.System import|from \\.Levelsystem import|from \\.Lasersystem import|from \\.Bfield import" MoleCool/__init__.py`
- `rg -n "def (calc_rateeqs|calc_OBEs|initialize_N0|_identify_iter_params|_verify_calculation|check_config)" MoleCool/System.py`
- `rg -n "def (add|add_sidebands|getarr|check_config|_identify_iter_params|_get_polarization_comps)" MoleCool/Lasersystem.py`
- `rg -n "def (add_electronicstate|add_all_levels|load_states|check_config)" MoleCool/Levelsystem.py`
- `rg -n "def (save_object|open_object|multiproc)" MoleCool/tools/__init__.py`
- `rg -n "def (ode0_rateeqs_jit|ode1_rateeqs_jit|ode1_OBEs_opt4)" MoleCool/tools/ODEs.py`

## Suggested source entry points (function-level)
- `MoleCool/__init__.py`: canonical API surface and convenience exports (`np`, constants, plotting helpers).
- `MoleCool/System.py`: simulation orchestration (`calc_rateeqs`, `calc_OBEs`), setup validation (`check_config`), and post-run verification (`_verify_calculation`).
- `MoleCool/Levelsystem.py`: state construction (`add_electronicstate`, `add_all_levels`, `load_states`) and level validation.
- `MoleCool/Lasersystem.py`: laser creation (`add`, `add_sidebands`), array extraction (`getarr`), and polarization parsing (`Laser._get_polarization_comps`).
- `MoleCool/Bfield.py`: magnetic-field setup (`turnon`) and remix matrix generation (`get_remix_matrix`).
- `MoleCool/tools/__init__.py`: persistence (`save_object`, `open_object`) and sweep execution (`multiproc`, `Results_OBEs_rateeqs`).
- `MoleCool/tools/ODEs.py`: low-level ODE kernels for debugging solver behavior or performance regressions.
- `MoleCool/run_examples.py`: CLI orchestration for scripted API smoke checks.

## Behavior checks
1. Import-surface sanity check
```bash
python - <<'PY'
from MoleCool import System, Lasersystem, Levelsystem, Bfield, np, save_object, open_object
print(System.__name__, Lasersystem.__name__, Levelsystem.__name__, Bfield.__name__)
print(np.__name__)
PY
```
Checkpoint: all imports resolve without errors.

2. API + serialization round-trip
```bash
python - <<'PY'
from pathlib import Path
from MoleCool import System, save_object, open_object

fname = Path('/tmp/molecool_api_skill_check')
system = System(load_constants='138BaF', verbose=False)
system.levels.add_all_levels(v_max=0)
system.lasers.add(lamb=860e-9, P=20e-3, pol='lin')
system.calc_rateeqs(t_int=3e-6, verbose=False)

assert system.success, system.message
save_object(system, filename=str(fname))
loaded = open_object(str(fname))
print('loaded_success:', loaded.success)
print('has_N:', hasattr(loaded, 'N'))
(fname.with_suffix('.pkl')).unlink(missing_ok=True)
PY
```
Checkpoint: loaded object reports `loaded_success: True` and contains `N`.

3. Optional sweep-path check
- Use an iterable laser parameter (for example an array for one laser attribute) and verify `system.results.iters` plus `system.results.vals` are populated after `calc_rateeqs(...)` or `calc_OBEs(...)`.
- Checkpoint: iteration keys match intended sweep dimensions.
