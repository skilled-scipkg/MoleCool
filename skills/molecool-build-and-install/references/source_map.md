# MoleCool source map: Build and Install

Generated from source roots:
- `MoleCool`
- `pyproject.toml`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "requires-python|dependencies|optional-dependencies" pyproject.toml`
- `rg -n "def (main|run_example_scripts)|--name|--out|--type|--show" MoleCool/run_examples.py`
- `rg -n "__version__|importlib.metadata|_version" MoleCool/__init__.py`
- `rg -n "def check_config" MoleCool/System.py MoleCool/Lasersystem.py MoleCool/Levelsystem.py`

## Suggested source entry points (function-level)
- `pyproject.toml`: `[project] requires-python`, runtime `dependencies`, and contributor extras in `[project.optional-dependencies]`.
- `MoleCool/__init__.py`: version lookup fallback and top-level imports used by install smoke tests.
- `MoleCool/run_examples.py`: `main`, `run_example_scripts`, and CLI option handling for install verification.
- `MoleCool/System.py`: `System.check_config` for runtime configuration failures seen right after installation.
- `MoleCool/Lasersystem.py`: `Lasersystem.check_config` for invalid or empty laser setups.
- `MoleCool/Levelsystem.py`: `Levelsystem.check_config` for missing level definitions.

## Simulation readiness checks
1. Environment/import check
```bash
python -c "import MoleCool; print(MoleCool.__version__)"
```
Checkpoint: command exits cleanly and prints a version string.

2. Example CLI discoverability
```bash
python -m MoleCool.run_examples -h
```
Checkpoint: help output includes `--name`, `--out`, `--type`, and `--show`.

3. Fast smoke suite
```bash
python -m MoleCool.run_examples --name fast
```
Checkpoint: summary table reports `Success` for each fast script.

4. Figure-output check
```bash
python -m MoleCool.run_examples --name plot_Simple3+1.py --out _skill_check_plots --type png
```
Checkpoint: at least one PNG file is written under `_skill_check_plots`.
