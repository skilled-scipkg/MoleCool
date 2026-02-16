# MoleCool source map: Substitutions

Generated from source roots:
- `doc/source`
- `MoleCool`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Fast source navigation
- `rg -n "^\.\. \|[A-Za-z0-9_]+\| (replace::|image::)" doc/source/substitutions.rst`
- `rg -n "\.\. include:: .*substitutions\.rst" doc/source`
- `rg -n "\|System\||\|Levelsystem\||\|Lasersystem\||\|Laser\||\|Bfield\||\|arxiv_badge\|" doc/source`
- `rg -n "class (System|Levelsystem|Lasersystem|Laser|Bfield)" MoleCool/System.py MoleCool/Levelsystem.py MoleCool/Lasersystem.py MoleCool/Bfield.py`

## Suggested source entry points (function-level)
- `doc/source/substitutions.rst`: single source of substitution token definitions.
- `doc/source/index.rst`: root-page usage of `|arxiv_badge|` and include wiring.
- `doc/source/examples.rst`: examples-page usage of substitutions and include wiring.
- `doc/source/user_guide/introduction.rst`: user-guide include path pattern (parent-relative substitutions include).
- `doc/source/user_guide/getting_started.rst`: heavy use of class substitution tokens.
- `MoleCool/System.py`, `MoleCool/Levelsystem.py`, `MoleCool/Lasersystem.py`, `MoleCool/Bfield.py`: class targets referenced by substitutions.

## Documentation validation checks
1. Token-definition integrity
```bash
rg -n "^\.\. \|[A-Za-z0-9_]+\| (replace::|image::)" doc/source/substitutions.rst
```
Checkpoint: all expected tokens are defined once in the canonical file.

2. Include wiring integrity
```bash
rg -n "\.\. include:: .*substitutions\.rst" doc/source
```
Checkpoint: each document using substitutions includes the file with a correct relative path.

3. Optional docs build check
```bash
sphinx-build -b html doc/source _build/skill_doc_check
```
Checkpoint: no unresolved-substitution warnings in build output.
