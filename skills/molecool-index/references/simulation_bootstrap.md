# MoleCool simulation bootstrap

Use this quick sequence when starting from scratch.

## 1) Environment and install
```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -e .
```

Validation checkpoint:
- `python -c "import MoleCool; print(MoleCool.__version__)"` runs without import errors.

## 2) Package smoke check
```bash
python -m MoleCool.run_examples --name fast
```

Validation checkpoint:
- Summary table reports `Success` for all fast scripts.

## 3) First custom simulation
```bash
python - <<'PY'
from MoleCool import System

system = System(load_constants='138BaF', verbose=False)
system.levels.add_all_levels(v_max=0)
system.lasers.add_sidebands(
    lamb=859.83e-9,
    P=20e-3,
    sidebands=[-2, -1, 1, 2],
    ratios=[0.8, 1.0, 1.0, 0.8],
    mod_freq=39.33e6,
    offset_freq=19e6,
)
system.Bfield.turnon(strength=5e-4, direction=[1, 1, 1])
system.calc_rateeqs(t_int=8e-6, magn_remixing=True, verbose=False)

print("success:", system.success)
print("population_sum:", float(system.N[:, -1].sum()))
PY
```

Validation checkpoint:
- `success: True`
- `population_sum` is close to `1.0`.

## 4) Route to deeper skill
- Build/install issues: `skills/molecool-build-and-install/SKILL.md`
- API/scripting issues: `skills/molecool-api-and-scripting/SKILL.md`
- Workflow/modeling issues: `skills/molecool-user-guide/SKILL.md`
- Example adaptation: `skills/molecool-examples-and-tutorials/SKILL.md`
