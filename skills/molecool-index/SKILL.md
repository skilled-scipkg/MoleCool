---
name: molecool-index
description: This skill should be used when users ask how to use MoleCool and a specific topic skill must be selected before deeper source inspection.
---

# MoleCool Skills Index

## Route the request
- Classify the request into one topic skill first.
- Keep answers practical: provide runnable commands, concrete inputs, and quick validation checkpoints.
- For first-run simulation setup, start with `references/simulation_bootstrap.md`.

## Generated topic skills
- `molecool-examples-and-tutorials`: worked examples, gallery scripts, and adaptation patterns.
- `molecool-getting-started`: first environment setup and first simulation run.
- `molecool-api-and-scripting`: API usage, object wiring, automation, and serialization.
- `molecool-build-and-install`: installation mode selection and install verification.
- `molecool-user-guide`: deeper modeling workflows and subsystem behavior.
- `molecool-substitutions`: Sphinx substitution tokens and include wiring.

## Documentation-first inputs
- `doc/source`

## Tutorials and examples roots
- `MoleCool/Examples`

## Escalate only when needed
- Start from the selected topic skill's primary docs in its `SKILL.md`.
- If needed, read that topic's documentation map file listed below.
- If docs are still insufficient, inspect that topic's source map file listed below.
- Use targeted source search with symbols from the user request.

## Topic map paths
- `skills/molecool-build-and-install/references/doc_map.md`
- `skills/molecool-build-and-install/references/source_map.md`
- `skills/molecool-getting-started/references/doc_map.md`
- `skills/molecool-getting-started/references/source_map.md`
- `skills/molecool-user-guide/references/doc_map.md`
- `skills/molecool-user-guide/references/source_map.md`
- `skills/molecool-api-and-scripting/references/doc_map.md`
- `skills/molecool-api-and-scripting/references/source_map.md`
- `skills/molecool-examples-and-tutorials/references/doc_map.md`
- `skills/molecool-examples-and-tutorials/references/source_map.md`
- `skills/molecool-substitutions/references/doc_map.md`
- `skills/molecool-substitutions/references/source_map.md`
