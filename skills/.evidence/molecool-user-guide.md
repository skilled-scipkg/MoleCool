# Evidence: molecool-user-guide

## Primary docs
- `doc/source/user_guide/index.rst`
- `doc/source/user_guide/getting_started.rst`

## Primary source entry points
- `skills/molecool-user-guide/references/doc_map.md`
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
- Access its subsystems
- single laser at 860 nm with linear polarization and 20 mW of power:
- multiple laser objects with circular polarization are added at once
- initiate empty level system
- add electronic ground ('gs') and excited ('exs') state
- add single quantum states
- print all defined states with their quantum numbers
- this basically iteratively calls e.g.
- set wavelengths in nm
- same as normal array indexing:

## Executable command hints
- (none extracted)

## Warnings and pitfalls
- The adaptive LSODA integrator provides excellent stability and performance,
- convergence of Monte Carlo statistics.
