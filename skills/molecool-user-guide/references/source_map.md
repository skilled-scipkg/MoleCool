# MoleCool source map: User Guide

Generated from source roots:
- `MoleCool`
- `MoleCool/Examples`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "def (calc_rateeqs|calc_OBEs|calc_Rabi_freqs|plot_spectrum|draw_levels|check_config|_verify_calculation)" MoleCool/System.py`
- `rg -n "def (add_all_levels|add_electronicstate|calc_all|print_properties|plot_transition_spectrum|check_config)" MoleCool/Levelsystem.py`
- `rg -n "def (add|add_sidebands|get_intensity_func|plot_spectrum|check_config)" MoleCool/Lasersystem.py`
- `rg -n "def (turnon|turnon_earth|get_remix_matrix)" MoleCool/Bfield.py`
- `rg -n "class (Molecule|ElectronicStateConstants)|def export_OBE_properties" MoleCool/spectra.py`

## Suggested source entry points (function-level)
- `MoleCool/System.py`: `System.calc_rateeqs`, `System.calc_OBEs`, `System.calc_Rabi_freqs`, `System.plot_spectrum`, `System.draw_levels`, `System._verify_calculation`.
- `MoleCool/Levelsystem.py`: `Levelsystem.add_electronicstate`, `Levelsystem.add_all_levels`, `Levelsystem.calc_all`, `Levelsystem.print_properties`, `ElectronicState.load_states`, `ElectronicGrState.add_lossstate`.
- `MoleCool/Lasersystem.py`: `Lasersystem.add`, `Lasersystem.add_sidebands`, `Lasersystem.get_intensity_func`, `Lasersystem.plot_spectrum`, `Lasersystem.check_config`.
- `MoleCool/Bfield.py`: `Bfield.turnon`, `Bfield.turnon_earth`, `Bfield.get_remix_matrix`.
- `MoleCool/spectra.py`: `Molecule`, `ElectronicStateConstants`, and `export_OBE_properties` bridge between spectroscopy fits and dynamics constants.
- `MoleCool/Examples/core/YbOH.py`: advanced loss-state/property update pattern used in realistic workflows.

## Simulation readiness checks
1. User-guide style assembly check
```bash
python - <<'PY'
from MoleCool import System

system = System(load_constants='138BaF', verbose=False)
system.levels.add_all_levels(v_max=0)
system.lasers.add(lamb=860e-9, P=20e-3, pol='lin')
system.Bfield.turnon(strength=5e-4, direction=[0, 0, 1], angle=60)
system.calc_rateeqs(t_int=6e-6, magn_remixing=True, verbose=False)

print("success:", system.success)
print("message:", system.message)
PY
```
Checkpoint: `success` is `True` and `message` stays empty or informational.

2. Coherent-dynamics (OBE) spot check
```bash
python - <<'PY'
from MoleCool import System

system = System(load_constants='138BaF', verbose=False)
system.levels.add_all_levels(v_max=0)
system.lasers.add(lamb=860e-9, P=20e-3, pol='lin')
system.Bfield.turnon(strength=5e-4, direction=[1, 1, 1])
system.calc_OBEs(t_int=2e-6, dt=2e-9, magn_remixing=True, verbose=False)

print("success:", system.success)
print("population_sum:", float(system.N[:, -1].sum()))
PY
```
Checkpoint: `success` is `True` and final population sum stays close to 1.

3. Transition/laser addressing sanity check
- After setup, run `system.plot_spectrum(...)` and verify intended laser frequencies overlap the intended transitions.
- Checkpoint: expected transition blocks are addressed; no obvious off-resonant-only configuration.
