# HR Attrition Classification

Predicting employee attrition from HR data and explaining its drivers with SHAP.

## Context

This project is part of the OpenClassrooms **AI Engineer** training program.

TechNova Partners, a fictional IT consulting firm, faces an unusually high
attrition rate. The HR team wants to **understand the potential causes of
resignations**.

Three extracts are provided by the HR team: the HR information system (role,
age, salary, seniority), the annual performance reviews (ratings and
satisfaction scores), and the latest well-being survey, which flags the
employees who left the company.

The project is carried out in three steps:

1. An exploratory analysis of the three files, to highlight the key
   differences between the employees who left and those who stayed.
2. A classification model that scores each employee's probability of leaving.
3. An interpretation of this model with SHAP, to identify the potential causes
   of resignations.

## Requirements

- Python 3.12+
- [uv](https://docs.astral.sh/uv/)

## Setup

```sh
uv sync
```

Then open the notebooks with the `.venv` kernel (in VS Code: *Select Kernel* →
*Python Environments* → `.venv`).

### Development setup

To work on the code after cloning, install the ruff hooks (lint and format,
notebooks included) and the nbstripout git filter:

```sh
uv run pre-commit install

uv run nbstripout --install --attributes .gitattributes
NB="$(pwd)/.venv/bin/python -m nbstripout --keep-output --keep-count"
git config filter.nbstripout.clean "$NB"
git config diff.ipynb.textconv "$NB -t"
git config filter.nbstripout.extrakeys "metadata.kernelspec metadata.language_info.version"
```

The filter strips the metadata that the editor rewrites (kernel, Python
version) from the committed notebooks and from the diffs, but it keeps the
outputs and the execution counts, because the notebooks are deliverables.

- `.gitattributes` is versioned and applies the filter to every notebook. The
  filter itself lives in `.git/config`, which is not versioned: run these
  commands once after cloning.
- `nbstripout --install` does not record any option, hence the `git config`
  lines. Run them again after any new `--install`.
- `git status` may still list a notebook that the editor only re-saved. Its
  `git diff` is empty, and `git add` clears it.
- VS Code compares notebooks without the filter, so its diff view still shows
  the metadata changes. To hide them, add `"notebook.diff.ignoreMetadata": true`
  to the VS Code `settings.json`.

## Data

The dataset is made of three HR extracts of 1,470 employees each, provided by
OpenClassrooms with the project brief:

- **HR information system** (`extrait_sirh.csv`)
- **Annual performance reviews** (`extrait_eval.csv`)
- **Annual well-being survey** (`extrait_sondage.csv`)

The data is not redistributed in this repository (`data/` is git-ignored)
because HR data is sensitive. To run the code, place the files so that you get:

```text
data/
└── raw/
    ├── extrait_sirh.csv
    ├── extrait_eval.csv
    └── extrait_sondage.csv
```

`data/processed/` is generated: `01_exploration` writes the joined dataset that
`02_modeling` loads, so run the notebooks in order.

## Project structure

```text
.
├── data/                   # git-ignored, see "Data"
├── notebooks/
│   ├── 01_exploration.ipynb   # cleaning, join, exploratory data analysis
│   └── 02_modeling.ipynb      # preparation, baselines, imbalance, threshold, tuning, SHAP
├── pyproject.toml
└── uv.lock
```
