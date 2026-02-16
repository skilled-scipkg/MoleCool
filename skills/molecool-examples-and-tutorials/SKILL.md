---
name: molecool-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in MoleCool; it prioritizes documentation references and then source inspection only for unresolved details.
---

# MoleCool: Examples and Tutorials

## High-Signal Playbook

### Route Conditions
- Use this skill to pick, run, and adapt curated MoleCool examples (`doc/source/examples.rst`, `MoleCool/Examples`).
- Route environment/install failures to `molecool-build-and-install`.
- Route class behavior interpretation to `molecool-user-guide`.
- Route low-level signature/API uncertainty to `molecool-api-and-scripting`.

### Triage Questions
- Is the target a fast smoke check, a long physics run, or figure regeneration?
- Which model family is needed (rate equations, OBEs, spectra, trajectories)?
- Do you need output files (`--out`, `--type`) or interactive figures (`--show`)?
- Is this a single script run or a gallery sweep?
- Do parameters need to be production-realistic or minimal-for-debug?
- Is runtime budget local-laptop or HPC-like?

### Canonical Workflow
1. Start from gallery docs (`core`, `misc`) to choose a scenario close to the user request (`doc/source/examples.rst`, `MoleCool/Examples/*/GALLERY_HEADER.rst`).
2. Use `python -m MoleCool.run_examples -h` to discover valid script names (`MoleCool/run_examples.py`).
3. Run `--name fast` for quick validation, then run a specific script for focused adaptation.
4. Save generated figures with `--out <dir> --type png` when reproducible artifacts are required (`MoleCool/run_examples.py`).
5. Adapt parameters directly inside a copied example script (constants, sidebands, solver settings, steady-state knobs).
6. For heavy runs, use scripts that already include multiprocessing and checkpoint/save patterns (`MoleCool/Examples/core/*.py`).

### Minimal Working Example
```bash
python -m MoleCool.run_examples -h
python -m MoleCool.run_examples --name fast
python -m MoleCool.run_examples --name plot_SimpleTest1_BaF.py --out _plots --type png
```

```python
from MoleCool import System, np

system = System(description='SimpleTest1_BaF', load_constants='138BaF')
for lamb in np.array([859.830, 895.699, 897.961]) * 1e-9:
    system.lasers.add_sidebands(
        lamb=lamb,
        P=20e-3,
        pol='lin',
        offset_freq=19e6,
        mod_freq=39.33e6,
        sidebands=[-2, -1, 1, 2],
        ratios=[0.8, 1, 1, 0.8],
    )
system.levels.add_all_levels(v_max=2)
system.calc_rateeqs(t_int=20e-6)
```

### Pitfalls and Fixes
- Invalid `--name` raises `ValueError`: query `-h` output first (`MoleCool/run_examples.py`).
- Running long examples as smoke tests wastes time: use `--name fast` first (`MoleCool/run_examples.py`).
- Forgetting `if __name__ == '__main__':` in heavy custom scripts can break multiprocessing patterns (see core examples).
- Iterative scripting without `del system.lasers[:]` can accidentally accumulate lasers (pattern shown in multiple examples).
- Copying advanced examples without checking constants/state prep can silently produce misleading physics (validate `print_properties` and spectrum plots).

### Convergence and Validation Checks
- Require all script entries to report `Success` in the built-in summary (`MoleCool/run_examples.py`).
- Validate saved figures exist and are non-empty when using `--out`.
- For adapted scripts, inspect `system.success` and normalized final populations.
- For steady-state OBEs examples, spot-check sensitivity to `steadystate['condition']`, `dt`, and `freq_clip_TH`.
- Cross-check one adapted example against gallery defaults before broad sweeps.

## Scope
- Handle questions about worked examples, tutorials, and cookbook usage.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/source/examples.rst`
- `MoleCool/Examples/misc/GALLERY_HEADER.rst`
- `MoleCool/Examples/core/GALLERY_HEADER.rst`

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
- `MoleCool/run_examples.py` (script discovery, `fast` vs `long`, per-script execution, output capture)
- `MoleCool/Examples/core/optcycl_3+1levels.py` (minimal OBE workflow)
- `MoleCool/Examples/misc/plot_SimpleTest1_BaF.py` (multi-sideband + rate-equation baseline)
- `MoleCool/Examples/misc/plot_SimpleTest2Traj_BaF.py` (trajectory + position-dependent rate equations)
- `MoleCool/Examples/core/cooling_forces.py` and `MoleCool/Examples/core/cooling_forces_RaF.py` (steady-state OBE force scans)
- Prefer targeted source search (for example: `rg -n "calc_OBEs|calc_rateeqs|add_sidebands" MoleCool/Examples`).
