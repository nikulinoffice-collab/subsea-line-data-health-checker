# Subsea Line Data Health Checker for AVEVA E3D

A compact portfolio project that demonstrates how a small AVEVA E3D subsea line model can be exported, checked in Python, and reported through structured Excel-based data quality outputs.

---

## Project Overview

This project is a small engineering automation case built around a compact AVEVA E3D piping model.

The workflow is designed to show more than 3D modeling skills. It demonstrates understanding of:
- E3D object attributes
- design data structure
- export normalization
- Python-based validation logic
- Excel reporting for model data quality

The project focuses on a small offshore/subsea-oriented line section so that the scope remains realistic for a portfolio case while still being rich enough for engineering checks.

---

## Objective

The main objective is to build a simple but credible data quality checking workflow for AVEVA E3D model data.

The workflow includes:
1. Defining a compact E3D line model
2. Exporting object data from E3D
3. Normalizing the export into structured CSV and Excel files
4. Validating key attributes with Python
5. Producing engineering-style Excel reports with errors and summaries

---

## Repository Structure

```text
subsea-line-data-health-checker/
├── README.md
├── requirements.txt
├── .gitignore
├── docs/
│   ├── project-roadmap.md
│   ├── portfolio-case-study.md
│   ├── data-architecture-note.md
│   ├── excel-design-notes.md
│   └── screenshots/
│       ├── e3d-model-overview.png
│       ├── e3d-line-attributes.png
│       ├── e3d-db-or-report-view.png
│       └── final-excel-report.png
├── data/
│   ├── input/
│   │   ├── raw/
│   │   │   ├── e3d_line_export.csv
│   │   │   └── e3d_line_export.xlsx
│   │   ├── reference/
│   │   │   ├── validation_rules.xlsx
│   │   │   └── project_data_template.xlsx
│   │   └── working/
│   │       └── project-data.csv
│   ├── output/
│   │   ├── filtered/
│   │   │   └── filtered_pipes.xlsx
│   │   ├── errors/
│   │   │   ├── attribute_errors.xlsx
│   │   │   └── error-log.csv
│   │   └── reports/
│   │       └── model_health_summary.xlsx
│   └── archive/
│       └── README.md
├── scripts/
│   ├── filter_pipes.py
│   ├── attribute_checker.py
│   ├── report_builder.py
│   ├── config.py
│   └── utils.py
├── samples/
│   ├── clean/
│   │   └── demo_line_clean.csv
│   ├── faulty/
│   │   └── demo_line_faulty.csv
│   └── demo_line_description.txt
└── excel/
    ├── Objects.xlsx
    ├── Rules.xlsx
    ├── Checks.xlsx
    └── offshore-line-data-health-check.xlsx
```

---

## Project Scope

The project is intentionally small.

The demo scope is based on one compact subsea/pipeline line section that may include:
- pipe segments
- elbows
- flanges
- valve
- supports
- nozzle or tie-in connection

This keeps the project manageable while still allowing useful validation logic around line consistency, materials, dimensions, specification data, and coating requirements.

---

## Input Data

The project uses several input layers.

### 1. E3D raw exports
Located in:
- `data/input/raw/e3d_line_export.csv`
- `data/input/raw/e3d_line_export.xlsx`

These files represent the initial data extracted from AVEVA E3D.

### 2. Reference and rule files
Located in:
- `data/input/reference/validation_rules.xlsx`
- `data/input/reference/project_data_template.xlsx`

These files define the expected structure, rule references, and validation assumptions.

### 3. Working normalized dataset
Located in:
- `data/input/working/project-data.csv`

This file acts as the normalized working dataset used by the Python scripts.

---

## Excel Design Layer

The `excel/` folder contains the structured Excel logic behind the project.

Files:
- `Objects.xlsx` — object register and engineering element list
- `Rules.xlsx` — validation rules and allowable logic
- `Checks.xlsx` — detailed check definitions
- `offshore-line-data-health-check.xlsx` — integrated Excel-based validation workbook

This Excel layer helps bridge E3D object data and Python validation logic.

---

## Validation Logic

The validation logic is intentionally simple, practical, and portfolio-friendly.

Typical checks include:
- missing `ElementID`
- missing `LineID`
- missing `Material`
- missing `SpecName` or `PipeClass`
- empty `NominalDiameter`
- empty or suspicious `WallThickness`
- missing `CoatingType` for subsea-related elements
- missing `Rating` where required
- inconsistent line-level attributes
- mismatch between expected objects and exported objects

The goal is not to recreate a full enterprise rule engine, but to demonstrate clear engineering reasoning through compact and believable data quality checks.

---

## Python Scripts

### `filter_pipes.py`
Reads source data and creates a filtered subset of relevant piping elements.

Typical filtering logic:
- keep only relevant element types
- filter by `LineID`
- optionally filter by material, diameter, or status
- export filtered results to `data/output/filtered/filtered_pipes.xlsx`

### `attribute_checker.py`
Runs the main attribute-level validation checks.

Expected output:
- `data/output/errors/attribute_errors.xlsx`
- `data/output/errors/error-log.csv`

### `report_builder.py`
Builds the final report package.

Expected output:
- `data/output/reports/model_health_summary.xlsx`

Typical report sheets may include:
- Clean Data
- Errors
- Summary
- By Line

### `config.py`
Stores reusable configuration such as file paths, required fields, allowed values, and rule parameters.

### `utils.py`
Contains helper functions used across the validation scripts.

---

## Sample Data

The `samples/` folder contains small demo datasets for testing and presentation.

Files:
- `samples/clean/demo_line_clean.csv`
- `samples/faulty/demo_line_faulty.csv`
- `samples/demo_line_description.txt`

The clean sample demonstrates expected structure.

The faulty sample is intended to include realistic data quality issues such as:
- missing material
- missing line ID
- invalid coating
- suspicious wall thickness
- missing rating
- incorrect class or specification field

---

## Output Reports

The project produces three main output groups.

### 1. Filtered data
Located in:
- `data/output/filtered/filtered_pipes.xlsx`

### 2. Error logs
Located in:
- `data/output/errors/attribute_errors.xlsx`
- `data/output/errors/error-log.csv`

### 3. Final summary report
Located in:
- `data/output/reports/model_health_summary.xlsx`

These outputs are intended to show both detailed validation findings and a higher-level overview of model data health.

---

## Documentation

The `docs/` folder contains the narrative and architecture side of the project.

Files:
- `project-roadmap.md` — development plan and phased progression
- `portfolio-case-study.md` — employer-facing explanation of the project value
- `data-architecture-note.md` — summary of project, MDB, DB, and data structure understanding
- `excel-design-notes.md` — explanation of the Excel validation design
- `screenshots/` — visual evidence linking E3D modeling, attributes, export views, and final reporting

This documentation is important because the project is meant to demonstrate engineering thinking, not only code execution.

---

## Technologies Used

- AVEVA E3D
- AVEVA Administration
- Python
- pandas
- openpyxl
- xlsxwriter
- Excel

---

## How to Run

1. Clone the repository
2. Create a Python virtual environment
3. Install dependencies from `requirements.txt`
4. Place or update the working input files in `data/input/`
5. Run the scripts in sequence:
   - `filter_pipes.py`
   - `attribute_checker.py`
   - `report_builder.py`

Example:

```bash
pip install -r requirements.txt
python scripts/filter_pipes.py
python scripts/attribute_checker.py
python scripts/report_builder.py
```

---

## Portfolio Value

This project is designed as a compact engineering automation case for portfolio use.

It demonstrates:
- understanding of AVEVA E3D object data
- awareness of project and database structure
- ability to normalize exports into usable datasets
- practical Python validation logic
- clear Excel-based reporting for engineering data quality

A stronger portfolio statement for this project is:

> Built a compact AVEVA E3D data quality workflow for a subsea line model, including object attribute mapping, database structure understanding, CSV/Excel export normalization, and Python-based validation reporting.

---

## Current Development Status

Current repository focus:
- establishing clean repository structure
- preparing documentation
- organizing source and output data folders
- formalizing validation logic
- preparing Python automation scripts

Planned next steps:
1. Finalize `requirements.txt`
2. Add initial script templates
3. Complete `project-roadmap.md`
4. Add architecture notes
5. Prepare normalized demo input files
6. Generate first filtered, error, and summary outputs

---

## Notes

This is a focused learning and portfolio project.

It is intentionally limited in scope and does not currently aim to include:
- direct E3D API integration
- SQL database implementation
- Power BI dashboards
- catalogue automation
- standalone user interface development

The purpose is to build a clean end-to-end workflow:
**E3D model -> export -> validation -> Excel report**