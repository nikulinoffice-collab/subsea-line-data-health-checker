# Project Roadmap

## Project Title

**Subsea Line Data Health Checker for AVEVA E3D**

## Project Description

Subsea Line Data Health Checker for AVEVA E3D is a compact demonstration project in which a small line section is modeled in AVEVA E3D, exported into tabular form, validated in Python, and reported through Excel-based data quality outputs.

The main purpose of the project is to demonstrate not only 3D modeling skills, but also an understanding of project structure, databases, and E3D object attributes. This is what separates a basic software user from someone who can support engineering automation.

## Project Goal

The goal of the project is to build a compact but credible engineering case that demonstrates:
- understanding of AVEVA E3D data structure;
- ability to work with object attributes;
- ability to export and normalize engineering data;
- ability to write Python-based quality checks;
- ability to produce clear Excel-based reports;
- ability to package the result as a portfolio-ready deliverable.

---

## Phase 1. Environment Setup

### 1.1 Define the project scope

The project should remain small and focused.

A single compact subsea or pipeline demo case is enough, for example one spool or one line section containing 8 to 20 objects such as:
- pipe;
- elbows;
- flange;
- valve;
- support;
- nozzle connection.

This scope is realistic for a portfolio project while still being rich enough for attribute-based data quality checks.

### 1.2 Create the repository structure

The project should be organized as an engineering mini-repository rather than a collection of temporary files.

Recommended structure:

```text
subsea-line-data-health-checker/
├── README.md
├── requirements.txt
├── data/
│   ├── input/
│   └── output/
├── docs/
│   └── screenshots/
├── scripts/
└── samples/
```

This is important because portfolio work is judged not only by code quality, but also by how clearly the engineering deliverable is packaged.

### 1.3 Prepare the Python environment

In PyCharm:
- create a new project;
- create a Python virtual environment;
- install the base libraries required for the first phase of the work.

Base dependencies:

```text
pandas
openpyxl
xlsxwriter
```

These libraries are needed for:
- reading CSV and Excel files;
- validating data;
- generating Excel reports.

---

## Phase 2. E3D Preparation

### 2.1 Review the E3D project environment

In AVEVA Administration 1.4, review:
- project access;
- users;
- overall database structure.

At this stage, full administration is not required. It is enough to identify:
- project name;
- connected databases;
- Design DB location;
- Catalogue and Specification data location;
- whether a training or test MDB is available.

### 2.2 Create a short data architecture note

Create the file:

```text
docs/data-architecture-note.md
```

In this file, briefly describe:
- Project;
- MDB;
- Design DB;
- Catalogue DB;
- Specification DB;
- System/Admin logic.

The purpose is not to rewrite product documentation, but to connect the project structure to the engineering workflow. For example:

> Here is the modeled object, here are its attributes, here is where the design data lives, and here is where catalog and specification choices come from.

### 2.3 Build a compact E3D demo case

In E3D Design, create a small test model containing:
- 1 equipment item or nozzle reference;
- 1 pipe run;
- 2 to 3 fittings;
- 1 valve;
- 2 to 3 supports;
- 1 flanged connection.

If subsea geometry is too complex for the first attempt, keep the geometry simple. The important part is that the object set remains engineering-realistic and contains multiple element types.

### 2.4 Populate key attributes intentionally

Do not only draw the model. Populate the attributes that will later be checked by Python scripts.

Minimum fields:
- ElementID
- ElementType
- LineID
- SpecName
- PipeClass
- NominalDiameter
- WallThickness
- Material
- CoatingType
- Rating
- Description
- Status

For subsea-oriented validation, the most useful fields are:
- Material;
- WallThickness;
- CoatingType;
- LineID.

These fields make it easier to demonstrate the engineering value of quality control checks.

---

## Phase 3. E3D Data Export

### 3.1 Produce the initial export

At least one tabular export is required from E3D:
- CSV;
- text listing;
- or an Excel table that can later be normalized manually.

If a perfect direct export is not available:
1. export the best available data;
2. manually normalize it into a clean CSV;
3. save it as:

```text
data/input/e3d_line_export.csv
```

For a portfolio project, this is acceptable because the key value lies in the workflow logic, not in an enterprise-grade connector.

### 3.2 Build a control Excel file

From the CSV, prepare:

```text
e3d_line_export.xlsx
```

Also prepare:

```text
validation_rules.xlsx
```

This file should contain simple validation rules such as:
- allowed materials;
- mandatory fields;
- valid DN range;
- mandatory coating for subsea elements;
- allowed status values.

---

## Phase 4. Python Implementation

### 4.1 Create `filter_pipes.py`

The first script should remain simple.

Its logic:
- read the input CSV or XLSX;
- keep only the relevant element types: PIPE, ELBOW, FLANGE, VALVE, SUPPORT;
- filter by LineID;
- optionally filter by NominalDiameter, Material, and Status;
- save the output as:

```text
filtered_pipes.xlsx
```

This provides an early working result.

### 4.2 Create `attribute_checker.py`

This is the main quality control script.

It should check:
- missing mandatory fields;
- missing LineID;
- missing Material;
- missing SpecName or PipeClass;
- invalid CoatingType for subsea elements;
- zero or suspicious WallThickness;
- missing Rating where required.

Output file:

```text
attribute_errors.xlsx
```

Required columns:
- ElementID
- ElementType
- FieldName
- IssueType
- IssueDescription
- Severity

### 4.3 Create `report_builder.py`

The third script should make the project feel complete.

It should generate:
- a `Clean Data` sheet;
- an `Errors` sheet;
- a `Summary` sheet with object counts, error counts, and distributions by type and field;
- optionally a `By Line` sheet grouped by LineID.

This step turns a collection of scripts into a portfolio-ready engineering deliverable.

---

## Phase 5. Engineering Validation Logic

### 5.1 Add intentional test errors

To make the project convincing, introduce 5 to 10 intentional issues into the demo dataset.

Examples:
- missing material;
- incorrect coating;
- missing LineID;
- incorrect pipe class;
- suspicious wall thickness;
- missing rating for a valve or flange.

This allows the project to demonstrate a clear before-and-after validation scenario.

### 5.2 Keep the rules simple and realistic

Do not try to recreate a full corporate rule engine in the first version.

Five to seven practical rules are enough, for example:
- all elements must have ElementID;
- all pipe, fitting, and valve elements must have LineID;
- all subsea elements must have CoatingType;
- NominalDiameter and WallThickness must not be empty for pipe elements;
- Material is mandatory for pipe, flange, and valve elements;
- SpecName is mandatory for piping items.

---

## Phase 6. Documentation

### 6.1 Write the README

The `README.md` file should contain seven short sections:
- Project Overview
- Objective
- Software Used
- Input Data
- Validation Logic
- Python Scripts
- Output Reports

It should also include the real software versions used in the project:
- AVEVA Everything3D 2.10;
- AVEVA Administration 1.4.0;
- PyCharm 2025.3.4;
- the local Python version.

### 6.2 Write `portfolio-case-study.md`

This file should answer a hiring manager’s question:

> What exactly was done in this project, and why is it valuable?

Recommended structure:
- Problem
- E3D Context
- Data Structure
- Validation Rules
- Python Workflow
- Example Findings
- Value for EPC / offshore projects

### 6.3 Prepare screenshots

Capture 3 to 4 screenshots:
- overall E3D model view;
- attributes of a selected element;
- listing, report, or table view;
- final Excel error report.

Screenshots make the project much more convincing because they visually connect 3D modeling with automation and reporting.

---

## Phase 7. Portfolio Positioning

### 7.1 Present it as a case, not a study exercise

A stronger final statement is:

> Built a compact AVEVA E3D data quality workflow for a subsea line model, including object attribute mapping, database structure understanding, CSV/Excel export normalization, and Python-based validation reporting.

This is much stronger than saying:

> studied E3D and Python

It presents the project as practical engineering data automation.

### 7.2 What is not required in the first version

The first version does not need:
- direct E3D API integration;
- a SQL database;
- Power BI dashboards;
- catalogue creation automation;
- a standalone user interface.

For the first three months, the following workflow is enough:
- E3D model;
- export;
- Python QC;
- report.

---

## Final Outcome

The final result of the project should be a compact but convincing engineering case that shows:
- understanding of AVEVA E3D project data;
- awareness of object attributes and validation logic;
- ability to structure and normalize exports;
- ability to automate checks in Python;
- ability to deliver clear Excel-based reporting;
- ability to present the work as a professional portfolio case.
