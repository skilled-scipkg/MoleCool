# Evidence: molecool-getting-started

## Primary docs
- `doc/source/user_guide/introduction.rst`
- `doc/source/installation.rst`

## Primary source entry points
- `skills/molecool-getting-started/references/doc_map.md`
- `MoleCool/__init__.py`
- `MoleCool/Bfield.py`
- `MoleCool/System.py`
- `MoleCool/spectra.py`
- `MoleCool/Levelsystem.py`
- `MoleCool/Lasersystem.py`
- `MoleCool/tools/__init__.py`
- `MoleCool/run_examples.py`
- `MoleCool/tools/ODEs.py`
- `MoleCool/tools/diststraj.py`
- `MoleCool/tools/dict2DF.py`

## Extracted headings
- Build level scheme
- Add laser configuration
- Magnetic field
- Dynamics simulations
- Create a virtual environment
- Activate the virtual environment
- Activate the virtual environment using:
- - Command Prompt / cmd.exe
- - PowerShell
- Create a new conda environment with Python 3.10 (recommended)
- Activate the environment

## Executable command hints
- python -m virtualenv -p python3.10 .venv
- python -m pip install --upgrade pip
- python -m MoleCool.run_examples

## Warnings and pitfalls
- .. important::
