---
name: molecool-substitutions
description: This skill should be used when users ask about substitutions in MoleCool; it prioritizes documentation references and then source inspection only for unresolved details.
---

# MoleCool: Substitutions

## High-Signal Playbook

### Route Conditions
- Use this skill for Sphinx substitution tokens and cross-reference macros used across docs (`doc/source/substitutions.rst`).
- Route simulation/modeling questions to `molecool-user-guide` or `molecool-getting-started`.
- Route runnable examples to `molecool-examples-and-tutorials`.

### Triage Questions
- Are you editing docs and seeing unresolved substitution warnings?
- Which token is needed (`|System|`, `|Levelsystem|`, `|Laser|`, `|arxiv_badge|`, etc.)?
- Is the page including `doc/source/substitutions.rst` with the correct relative path?
- Do you need a class cross-reference or an external badge/link?

### Canonical Workflow
1. Open `doc/source/substitutions.rst` and identify the canonical substitution token.
2. Ensure the target document includes a valid relative include path to `doc/source/substitutions.rst` (for example `.. include:: substitutions.rst` from `doc/source/index.rst`, or `.. include:: ../substitutions.rst` from `doc/source/user_guide/introduction.rst`).
3. Use existing substitution tokens for API class references instead of hardcoding full directive text.
4. Keep substitutions centralized in `doc/source/substitutions.rst` so names remain consistent across docs.
5. Rebuild docs and fix any unresolved substitution warnings before merging.

### Minimal Working Example
```rst
.. include:: ../substitutions.rst

The central entry point is |System|, which composes |Levelsystem|,
|Lasersystem|, and |Bfield|.
```

```rst
For package citation, use |arxiv_badge|.
```

### Pitfalls and Fixes
- Missing include directive causes unresolved substitutions: add the correct relative include to `doc/source/substitutions.rst`.
- Typo in token names (for example `|Lasersytem|`) fails silently in rendered intent: copy exact token names from `doc/source/substitutions.rst`.
- Re-declaring class substitutions in many files creates drift: keep single-source definitions in `doc/source/substitutions.rst`.
- Mixing substitution usage in code blocks instead of body text can produce confusing output; keep substitutions in prose/rst context.

### Convergence and Validation Checks
- Confirm all used tokens are defined in `doc/source/substitutions.rst`.
- Run a doc build and require zero unresolved substitution warnings.
- Spot-check rendered links for core classes (`|System|`, `|Levelsystem|`, `|Lasersystem|`, `|Bfield|`).

## Scope
- Handle questions about documentation grouped under the 'substitutions' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/source/substitutions.rst`

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
- `doc/source/substitutions.rst` (canonical token definitions)
- `doc/source/index.rst`, `doc/source/examples.rst`, `doc/source/user_guide/introduction.rst`, `doc/source/user_guide/getting_started.rst` (real usage patterns and include points)
- `MoleCool/System.py`, `MoleCool/Levelsystem.py`, `MoleCool/Lasersystem.py`, `MoleCool/Bfield.py` (targets of class cross-reference tokens)
- Prefer targeted source search (for example: `rg -n "\|System\||\.\. include:: .*substitutions" doc/source`).
