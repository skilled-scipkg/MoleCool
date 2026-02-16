---
name: molecool-user-guide
description: This skill should be used when users ask about user guide in MoleCool; it prioritizes documentation references and then source inspection only for unresolved details.
---

# MoleCool: User Guide

## High-Signal Playbook

### Route Conditions
- Use this skill for modeling workflow questions involving `System`, `Levelsystem`, `Lasersystem`, and `Bfield` (`doc/source/user_guide/getting_started.rst`).
- Route install/bootstrap problems to `molecool-build-and-install`.
- Route end-to-end runnable scripts to `molecool-examples-and-tutorials`.
- Route symbol-level signatures and class lists to `molecool-api-and-scripting`.

### Triage Questions
- Which molecule/constants set is being used (`load_constants='...'`) and which electronic states are needed?
- Are levels loaded from constants or manually specified with quantum numbers?
- Is the run rate-equation based, OBE based, or both?
- Do lasers require sidebands, chirps, or position-dependent Gaussian beams?
- Is magnetic remixing required, and with what field strength/angle?
- Is this a single-run scenario or a parameter sweep/multiprocessing workload?

### Canonical Workflow
1. Initialize `System(...)` and inspect subsystem handles (`system.lasers`, `system.levels`, `system.Bfield`) (`doc/source/user_guide/getting_started.rst`).
2. Build the level scheme via `add_electronicstate` + `load_states` or `add_all_levels` (`doc/source/user_guide/getting_started.rst`).
3. Configure lasers with `add(...)` or `add_sidebands(...)`, then inspect with `print(system.lasers)` (`doc/source/user_guide/getting_started.rst`).
4. Configure magnetic field (`Bfield.turnon(...)`) only if remixing/Zeeman effects are needed (`doc/source/user_guide/getting_started.rst`).
5. Run `calc_rateeqs(...)` for efficient long/trajectory cases or `calc_OBEs(...)` for coherent dynamics (`doc/source/user_guide/getting_started.rst`).
6. Review populations/forces/scattering outputs and plotting helpers (`plot_N`, `plot_F`, `plot_Nscatt`, `plot_all`) (`doc/source/user_guide/getting_started.rst`).
7. For spectral constants workflows, use `MoleCool.spectra` and feed extracted properties back to dynamics (`doc/source/user_guide/getting_started.rst`).

### Minimal Working Example
```python
from MoleCool import System

system = System(description='my_first_test', load_constants='138BaF')
system.levels.add_all_levels(v_max=0)

system.lasers.add(
    lamb=860e-9,
    P=20e-3,
    pol='lin',
)
system.Bfield.turnon(strength=5e-4, direction=[0, 0, 1], angle=60)

system.calc_rateeqs(t_int=20e-6, magn_remixing=True)
system.plot_N()
system.plot_F()
```

### Pitfalls and Fixes
- Modifying states after properties were initialized: add/delete states before property initialization (`ElectronicState.add`, `ElectronicState.__delitem__` in `MoleCool/Levelsystem.py`).
- `load_states` with missing constants: ensure `load_constants` is set or manually define quantum states (`ElectronicState.load_states`).
- Invalid quantum inputs (`|mF| > F`, bad datatypes) raise errors: validate quantum numbers up front (`ElectronicState.add`).
- Large B-field regime warning (`B > 10 G`) for linear Zeeman approximation: keep fields in valid regime or interpret cautiously (`Bfield.turnon` in `MoleCool/Bfield.py`).
- Trajectory mode without mass fails: use constants with mass metadata before `trajectory=True` (`System.check_config`).
- Adding loss states can require corresponding property rows (see YbOH example pattern for `vibrbranch`/`wavelengths` augmentation) (`MoleCool/Examples/core/YbOH.py`).

### Convergence and Validation Checks
- Print and inspect level/transition properties before long runs (`system.levels.print_properties()`) (`doc/source/user_guide/getting_started.rst`).
- Check solver verification flags/messages after each run (`system.success`, `system.message`) (`System._verify_calculation`).
- For steady-state OBEs, inspect `system.step` and convergence settings (`steadystate['condition']`, `steadystate['maxiters']`) (`System.calc_OBEs`).
- Validate physical trend coherence by comparing selected rate-equation and OBE outputs for the same setup.
- For transition-driven runs, inspect transition/laser overlay plots (`system.plot_spectrum(...)`) to verify addressing (`MoleCool/System.py`, `MoleCool/Lasersystem.py`).

## Scope
- Handle questions about documentation grouped under the 'user-guide' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/source/user_guide/index.rst`
- `doc/source/user_guide/getting_started.rst`

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
- `MoleCool/System.py` (`System.calc_rateeqs`, `System.calc_OBEs`, `System.calc_Rabi_freqs`, `System.plot_spectrum`)
- `MoleCool/Levelsystem.py` (`Levelsystem.add_electronicstate`, `Levelsystem.add_all_levels`, `ElectronicState.load_states`, `ElectronicGrState.add_lossstate`, `Levelsystem.check_config`)
- `MoleCool/Lasersystem.py` (`Lasersystem.add`, `Lasersystem.add_sidebands`, `Lasersystem.get_intensity_func`, `Lasersystem.plot_spectrum`)
- `MoleCool/Bfield.py` (`Bfield.turnon`, `Bfield.turnon_earth`, `Bfield.get_remix_matrix`)
- `MoleCool/spectra.py` (constants extraction and spectrum workflows for feedback into dynamics)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" MoleCool`).
