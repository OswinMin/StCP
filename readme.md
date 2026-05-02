# Code

This folder contains the core code and minimal datasets for reproducing the paper experiments.

## 1. Directory structure

```text
Code/
├─ Main/                 # Core methods and shared utilities
│  ├─ GLCP.py, SLCP.py, SCP.py, SSAE.py, RLCP.py
│  ├─ Predictor.py, QuanRegressor.py, engGenerator.py
│  ├─ Agents.py, tools.py, scoreFun.py, ...
├─ RealAnalysis/         # Real-data experiment entry scripts
│  ├─ procedure.py       # Main real-data experiment pipeline
│  ├─ Crime.py           # Communities & Crime experiment
│  ├─ Protein.py         # Protein structure experiment
│  ├─ Achieve.py         # STAR/Achievement experiment
│  ├─ Derma.py           # DermaMNIST experiment
│  ├─ Tissue.py          # TissueMNIST experiment
│  ├─ sum_tab.py         # Summarize real-data results into table text
│  └─ Para/              # Pretrained model checkpoints used by image experiments
├─ SimuAnalysis/         # Simulation experiments
│  ├─ config.py          # Global experiment config (dtypes, n/m grids, hyperparameters)
│  ├─ core.py            # Simulation core pipeline
│  ├─ run_shared.py      # Unified runner: one command per (dtype, n, m)
│  ├─ plot_lambda.py     # Plot lambda sensitivity
│  ├─ sum_tab.py         # Export simulation summary tables (for manuscript)
│  └─ shared_utils.py    # Shared paths / load-save helpers
└─ Dataset/              # Data files used by RealAnalysis and (partly) SimuAnalysis
```

## 2. What each part does

- **`Main/`**: algorithm implementations and helper classes used by both real-data and simulation code.
- **`RealAnalysis/`**: task-specific scripts that load data, define target/source split, and call `procedure.py`.
- **`SimuAnalysis/`**: configurable simulation framework (multi-DGP, shared result pool, plotting and table generation).
- **`Dataset/`**: local datasets required by scripts in `RealAnalysis/`.

## 3. Environment

Use Python 3.10+ (recommended). Typical dependencies:

- `numpy`, `scipy`, `pandas`, `scikit-learn`
- `matplotlib`, `seaborn`
- `torch`, `torchvision`
- `Pillow`, `xmltodict` (if using related image/xml utilities)

## 4. Running code

> Run commands from **`Code/`** root.

### 4.1 Simulation experiments

Run one simulation setting:

```bash
python SimuAnalysis/run_shared.py <dtype> <n> <m>
```

Example:

```bash
python SimuAnalysis/run_shared.py quad 30 500
```

Generate lambda-sensitivity plots (based on results configured in `SimuAnalysis/config.py`):

```bash
python SimuAnalysis/plot_lambda.py
```

Generate simulation tables:

```bash
python SimuAnalysis/sum_tab.py
```

### 4.2 Real-data experiments

Each dataset has an entry script. Examples:

```bash
python RealAnalysis/Crime.py
python RealAnalysis/Protein.py
python RealAnalysis/Achieve.py
python RealAnalysis/Derma.py
python RealAnalysis/Tissue.py
```

You can also pass script arguments (see defaults and `sys.argv` parsing in each file).

Generate real-data summary table text:

```bash
python RealAnalysis/sum_tab.py
```

## 5. Outputs

Scripts usually write outputs to sibling folders created on demand (e.g., `SimResult/`, `Log/`, `Figure/` under `Code/`).

## 6. Notes

- `SimuAnalysis/config.py` is the central place to adjust simulation grids and hyperparameters.
- `RealAnalysis/procedure.py` is the central pipeline for real-data experiments.
- If a dataset file is missing, place it under `Code/Dataset/` with the expected relative path used in the script.
