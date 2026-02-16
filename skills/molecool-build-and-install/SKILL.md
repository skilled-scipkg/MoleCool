---
name: molecool-build-and-install
description: This skill should be used when users ask about build and install in MoleCool; it prioritizes documentation references and then source inspection only for unresolved details.
---

# MoleCool: Build and Install

## High-Signal Playbook

### Route Conditions
- Use this skill for environment creation, install method choice (`pip`/`conda`/`git+pip`), editable installs, and installation verification (`doc/source/installation.rst`).
- Route first simulation setup questions to `molecool-getting-started`.
- Route API usage after install to `molecool-api-and-scripting` or `molecool-user-guide`.
- Route curated runnable scenarios to `molecool-examples-and-tutorials`.

### Triage Questions
- Which OS and shell are you using?
- Which Python version is active (`python --version`) and does it satisfy >=3.8 (<=3.10 recommended)?
- Do you need stable packages (`pip`/`conda`) or latest source (`git clone` + local install)?
- Are you contributing and therefore need editable mode + extras (`-e .[dev,doc]`)?
- Do you need a quick install smoke test only, or full example coverage with saved plots?

### Canonical Workflow
1. Create an isolated environment (`virtualenv` or `conda`) (`doc/source/installation.rst`).
2. Upgrade installer tooling (`python -m pip install --upgrade pip`) (`doc/source/installation.rst`).
3. Install stable package via `pip install MoleCool` or `conda install -c conda-forge MoleCool` (`doc/source/installation.rst`).
4. For latest source, clone repository and install locally (`pip install .`) (`doc/source/installation.rst`).
5. For contributors, use editable mode with extras: `pip install -e .[dev,doc]` (`doc/source/installation.rst`).
6. Verify installation with `python -m MoleCool.run_examples` and optional `-h`/`--name` flags (`doc/source/installation.rst`, `MoleCool/run_examples.py`).

### Minimal Working Example
```bash
python -m virtualenv -p python3.10 .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install MoleCool
python -m MoleCool.run_examples --name fast
```

```bash
# contributor/development mode
git clone https://www.github.com/LangenGroup/MoleCool
cd MoleCool
python -m pip install --upgrade pip
pip install -e .[dev,doc]
python -m MoleCool.run_examples --name plot_Simple3+1.py --out _example_plots --type png
```

### Pitfalls and Fixes
- Wrong Python version: use Python 3.10 for best compatibility (module requires >=3.8) (`doc/source/installation.rst`).
- Import confusion when cwd parent is named `MoleCool`: run from a proper environment path and avoid parent-folder shadowing (`doc/source/installation.rst`).
- Missing `git` for beta install path: either install git or use stable `pip`/`conda` method (`doc/source/installation.rst`).
- Invalid example name in verifier: `run_examples` raises `ValueError`; use `-h` to list valid names (`MoleCool/run_examples.py`).
- Assuming all examples are quick: prefer `--name fast` for smoke tests; long examples are intentionally heavier (`MoleCool/run_examples.py`).

### Convergence and Validation Checks
- Confirm import/version in active environment: `python -c "import MoleCool; print(MoleCool.__version__)"` (`MoleCool/__init__.py`).
- Run smoke suite: `python -m MoleCool.run_examples --name fast` and require all entries `Success` (`MoleCool/run_examples.py`).
- Validate example CLI discoverability: `python -m MoleCool.run_examples -h`.
- If saving figures, check expected files are emitted under `--out` path (`MoleCool/run_examples.py`).

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/source/index.rst`
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
- `MoleCool/run_examples.py` (`main`, `run_example_scripts`, CLI flags `--name`, `--out`, `--type`, `--show`)
- `MoleCool/__init__.py` (public exports and version resolution fallback)
- `MoleCool/System.py` (`System.check_config`) for runtime precondition failures after installation
- `pyproject.toml` (dependency and extras metadata)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" MoleCool pyproject.toml`).
