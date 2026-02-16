---
name: molecool-api-and-scripting
description: This skill should be used when users ask about api and scripting in MoleCool; it prioritizes documentation references and then source inspection only for unresolved details.
---

# MoleCool: API and Scripting

## High-Signal Playbook

### Route Conditions
- Use this skill for Python API usage, class-level scripting patterns, and automation around `System`, `Levelsystem`, `Lasersystem`, `Bfield`, and `spectra` (`doc/source/api.rst`).
- Route installation/packaging issues to `molecool-build-and-install`.
- Route conceptual workflow questions to `molecool-user-guide`.
- Route full runnable scripts and benchmark-like examples to `molecool-examples-and-tutorials`.

### Triage Questions
- Which API entry point is needed (`System`, standalone `Lasersystem`, standalone `spectra`)?
- Is the target a single simulation, parametric sweep, or reusable library wrapper?
- Are rates or OBEs required, and are steady-state settings needed?
- Will trajectories/position dependence be used?
- Do you need serialization/export of results (`tools.save_object`, `tools.open_object`)?
- Is the issue signature-level, numeric behavior-level, or performance-level?

### Canonical Workflow
1. Start from exported top-level API (`from MoleCool import System, Lasersystem, Levelsystem, Bfield`) (`MoleCool/__init__.py`, `doc/source/api.rst`).
2. Build a minimal object graph (`System` or standalone classes) and configure levels/lasers/B-field (`doc/source/user_guide/getting_started.rst`).
3. Run `calc_rateeqs(...)` or `calc_OBEs(...)` depending on incoherent vs coherent dynamics requirements (`MoleCool/System.py`).
4. For sweeps, pass iterable arrays (laser/system parameters) and rely on built-in multiprocessing path (`System._identify_iter_params`, `tools.multiproc`).
5. Inspect outputs via `system.N`, `system.results.vals`, plots, and verification flags (`System._verify_calculation`).
6. Persist and reload objects with tools helpers when building scripted pipelines (`MoleCool/tools/__init__.py`).

### Minimal Working Example
```python
from MoleCool import System

system = System(load_constants='138BaF')
system.levels.add_all_levels(v_max=1)
system.lasers.add_sidebands(
    lamb=859.83e-9,
    P=20e-3,
    sidebands=[-2, -1, 1, 2],
    ratios=[0.8, 1.0, 1.0, 0.8],
    mod_freq=39.33e6,
    offset_freq=19e6,
)
system.calc_rateeqs(t_int=20e-6, magn_remixing=True)
print(system.success, system.message)
```

### Pitfalls and Fixes
- Invalid polarization strings/types (`pol`) raise exceptions: use only `lin`, `sigmap`, or `sigmam` (`Laser._get_polarization_comps` in `MoleCool/Lasersystem.py`).
- Empty laser or level configuration triggers warnings/errors in solver prechecks (`Lasersystem.check_config`, `Levelsystem.check_config`).
- Mis-shaped vectors for `set_v0`/`set_r0` cause shape errors: ensure last dimension is length 3 (`System.__set_v0r0`).
- Wrong `N0` size raises `ValueError`: match level count exactly (or let initialization auto-build) (`System.initialize_N0`).
- `load_states` without constants dictionary fails: provide `load_constants` or define states manually (`ElectronicState.load_states`).
- Expecting direct power/intensity invariance while changing beam widths: `Laser.P`/`Laser.I` are linked via beam geometry (`MoleCool/Lasersystem.py`).

### Convergence and Validation Checks
- Require solver verification pass (`system.success is True`) and inspect `system.message` (`System._verify_calculation`).
- Validate normalized populations and non-negativity at final time (`System._verify_calculation`).
- For multiparameter runs, verify `system.results.vals` dimensions match iterated parameter shapes.
- If using `dt='auto'` in OBE, sanity-check resolved dynamics with a smaller fixed `dt` spot check (`System._evaluate`).
- Verify units when scripting detunings/chirps: `freq_shift`/`beta` are non-angular frequencies (`MoleCool/Lasersystem.py`).

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/source/api.rst`
- `doc/source/_templates/autosummary/class.rst`

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
- `MoleCool/__init__.py` (public API surface and re-exports)
- `MoleCool/System.py` (`System.calc_rateeqs`, `System.calc_OBEs`, `System.initialize_N0`, `System._identify_iter_params`, `System._verify_calculation`)
- `MoleCool/Levelsystem.py` (`Levelsystem.add_electronicstate`, `ElectronicState.add`, `ElectronicState.load_states`, `Levelsystem.calc_all`)
- `MoleCool/Lasersystem.py` (`Lasersystem.add`, `Lasersystem.add_sidebands`, `Lasersystem.getarr`, `Laser._get_polarization_comps`)
- `MoleCool/Bfield.py` (`Bfield.turnon`, `Bfield.Bvec_sphbasis`)
- `MoleCool/tools/__init__.py`, `MoleCool/tools/ODEs.py`, `MoleCool/tools/diststraj.py`, `MoleCool/tools/dict2DF.py`
- `MoleCool/run_examples.py` for CLI automation patterns
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" MoleCool`).
