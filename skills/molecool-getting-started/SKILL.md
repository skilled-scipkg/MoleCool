---
name: molecool-getting-started
description: This skill should be used when users ask about getting started in MoleCool; it prioritizes documentation references and then source inspection only for unresolved details.
---

# MoleCool: Getting Started

## High-Signal Playbook

### Route Conditions
- Use this skill for first-run setup, first simulation bootstrapping, and "is my environment healthy?" checks (`doc/source/installation.rst`, `doc/source/user_guide/introduction.rst`).
- Route package-manager and install-mode specifics to `molecool-build-and-install`.
- Route detailed class behavior and object wiring questions to `molecool-user-guide`.
- Route runnable script selection and adaptation to `molecool-examples-and-tutorials`.
- Route symbol-level API behavior to `molecool-api-and-scripting`.

### Triage Questions
- Which platform and env manager are you using (`virtualenv`, `conda`, system Python)?
- Are you targeting stable install (`pip`/`conda`) or editable development mode?
- Do you want a smoke test first (`python -m MoleCool.run_examples`) or direct custom simulation?
- Are you loading built-in constants (for example `138BaF`) or building states manually?
- Are you starting with rate equations, OBEs, or both?
- Do you need trajectory/position-dependent runs immediately?

### Canonical Workflow
1. Create and activate a dedicated Python environment (Python 3.10 recommended; module requires >=3.8) (`doc/source/installation.rst`).
2. Install MoleCool using `pip`, `conda`, or local clone (`doc/source/installation.rst`).
3. Run the built-in smoke tests via `python -m MoleCool.run_examples` (`doc/source/installation.rst`, `MoleCool/run_examples.py`).
4. Initialize `System(load_constants='138BaF')` and populate levels/lasers/B-field (`doc/source/user_guide/getting_started.rst`).
5. Run a short `calc_rateeqs(...)` or `calc_OBEs(...)` job (`doc/source/user_guide/introduction.rst`, `doc/source/user_guide/getting_started.rst`).
6. Validate with `system.plot_N()` plus population checks (`System._verify_calculation` in `MoleCool/System.py`).
7. Escalate to examples for realistic parameter scans (`doc/source/examples.rst`, `MoleCool/Examples`).

### Minimal Working Example
```bash
python -m virtualenv -p python3.10 .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install MoleCool
python -m MoleCool.run_examples --name fast
```

```python
from MoleCool import System

system = System(load_constants='138BaF')
system.levels.add_all_levels(v_max=0)
system.lasers.add_sidebands(
    lamb=859.83e-9,
    P=20e-3,
    offset_freq=19e6,
    mod_freq=39.33e6,
    sidebands=[-2, -1, 1, 2],
    ratios=[0.8, 1, 1, 0.8],
)
system.Bfield.turnon(strength=5e-4, direction=[1, 1, 1])
system.calc_rateeqs(t_int=8e-6, magn_remixing=True)
system.plot_N()
```

### Pitfalls and Fixes
- Import path confusion from a parent directory named `MoleCool`: run from an activated env and not from a conflicting parent path (`doc/source/installation.rst`).
- `load_states(...)` fails with no constants dictionary: instantiate with `load_constants=...` or add states manually (`MoleCool/Levelsystem.py`).
- Trajectory runs fail with missing mass: use constants that include mass before `trajectory=True` (`System.check_config` in `MoleCool/System.py`).
- Empty laser/state configs: add at least one laser and define levels before solver calls (`Lasersystem.check_config`, `Levelsystem.check_config`).
- Requesting unavailable vibrational states: loader may warn and copy `v=0` structure; verify imported levels before running (`ElectronicState.load_states` in `MoleCool/Levelsystem.py`).

### Convergence and Validation Checks
- Confirm smoke tests succeed: `python -m MoleCool.run_examples --name fast` (`doc/source/installation.rst`, `MoleCool/run_examples.py`).
- After each run, check `system.success` and `system.message` (`System._verify_calculation` in `MoleCool/System.py`).
- Verify final population normalization is close to 1 and no large negative populations (`System._verify_calculation` in `MoleCool/System.py`).
- For first physical sanity checks, compare a short rate-equation run against a short OBE run for trend consistency.
- For trajectory runs, confirm `system.v` and `system.r` exist and are physically plausible (`System.calc_rateeqs` with `trajectory=True`).

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/source/user_guide/introduction.rst`
- `doc/source/installation.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `MoleCool/Examples`

## Test references
- None discovered.

## Optional deeper inspection
- `MoleCool`

## Source entry points for unresolved issues
- `MoleCool/System.py` (`System.__init__`, `System.calc_rateeqs`, `System.calc_OBEs`, `System.check_config`, `System._verify_calculation`)
- `MoleCool/Levelsystem.py` (`Levelsystem.add_all_levels`, `Levelsystem.add_electronicstate`, `ElectronicState.load_states`, `ElectronicGrState.add_lossstate`)
- `MoleCool/Lasersystem.py` (`Lasersystem.add`, `Lasersystem.add_sidebands`, `Lasersystem.check_config`)
- `MoleCool/Bfield.py` (`Bfield.turnon`, `Bfield.reset`, `Bfield.get_remix_matrix`)
- `MoleCool/run_examples.py` (`main`, `run_example_scripts`)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" MoleCool`).
