# Excel Design Notes

## Purpose

This document explains the role of the Excel layer in the **Subsea Line Data Health Checker for AVEVA E3D** project.

The Excel files in this project are not only storage containers. They serve as a structured bridge between:
- AVEVA E3D exported data;
- manual engineering review;
- Python-based validation logic;
- final Excel reporting outputs.

The purpose of this design is to make the workflow understandable, portable, and easy to explain in a portfolio context.

---

## Why Excel Is Used in This Project

Excel is used because it is a familiar and practical engineering format for:
- reviewing exported model data;
- defining simple validation rules;
- comparing clean and faulty records;
- presenting validation findings in a readable form.

In this project, Excel is also useful because the goal is not to build a software product, but to demonstrate a realistic engineering data-quality workflow.

---

## Excel Layer Overview

The `excel/` folder contains the workbook layer that supports validation design and data review.

Files:
- `Objects.xlsx`
- `Rules.xlsx`
- `Checks.xlsx`
- `offshore-line-data-health-check.xlsx`

These files act as the structured design layer behind the Python validation workflow.

---

## Workbook Roles

### `Objects.xlsx`

This workbook is used as the object register.

Its purpose is to define or review the engineering object list that belongs to the compact subsea line case.

Typical content may include:
- ElementID;
- ElementType;
- LineID;
- SpecName;
- PipeClass;
- NominalDiameter;
- WallThickness;
- Material;
- CoatingType;
- Rating;
- Description;
- Status.

This workbook helps create a clear object inventory before or alongside the exported E3D dataset.

### `Rules.xlsx`

This workbook contains validation rule definitions.

Its role is to make the checking logic visible and editable without forcing all validation knowledge into Python code.

Typical rule categories may include:
- mandatory fields;
- allowed materials;
- expected coating rules;
- valid rating logic;
- allowed status values;
- expected class or specification logic.

This workbook acts as the rule reference layer of the project.

### `Checks.xlsx`

This workbook contains the detailed checks that connect object attributes to validation logic.

It is more operational than `Rules.xlsx`.

Typical content may include:
- check ID;
- field name;
- target element type;
- validation condition;
- expected value or allowable set;
- severity;
- issue description.

This workbook helps convert engineering expectations into clear validation checks that can later be implemented in Python.

### `offshore-line-data-health-check.xlsx`

This workbook acts as the combined or presentation-level Excel file.

Its role is to bring together:
- object review;
- validation logic;
- check outcomes;
- summary sheets;
- presentation-friendly reporting.

Depending on project evolution, this workbook may become the main report package or a master workbook used for demonstration.

---

## Relationship to Input Data

The Excel layer is linked to the exported and normalized project input data.

Relevant upstream files include:
- `data/input/raw/e3d_line_export.csv`
- `data/input/raw/e3d_line_export.xlsx`
- `data/input/reference/validation_rules.xlsx`
- `data/input/reference/project_data_template.xlsx`
- `data/input/working/project-data.csv`

The design idea is that exported E3D data is first collected, then normalized into a stable structure, and then compared against engineering expectations defined in Excel and Python.

---

## Relationship to Python Scripts

The Excel design is closely connected to the Python scripts in the `scripts/` folder.

### `filter_pipes.py`

This script reads the input data and produces a filtered subset of relevant piping elements.

Its output is intended to support a cleaner downstream validation step.

### `attribute_checker.py`

This script performs the main quality checks against the normalized dataset.

The design intent is that many of the checks executed in Python should already be understandable from the logic documented in `Rules.xlsx` and `Checks.xlsx`.

### `report_builder.py`

This script turns validation output into final report files.

The Excel design supports this by defining:
- which fields matter most;
- which issue categories should appear in reports;
- which sheets are useful for engineering review.

---

## Recommended Column Logic

To keep the workflow stable, the project should use a consistent set of core columns across CSV, Excel, and Python logic.

Recommended core columns:
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

Using the same naming convention across all workbooks and scripts reduces mapping errors and makes the project easier to explain and maintain.

---

## Recommended Sheet Logic

A practical Excel design for this project may include sheets such as:
- `Objects`
- `Rules`
- `Checks`
- `Clean Data`
- `Errors`
- `Summary`
- `By Line`

This sheet structure supports both engineering review and scripted reporting.

### `Objects`
Stores the object register or normalized object list.

### `Rules`
Stores business or engineering validation rules.

### `Checks`
Stores validation check definitions and issue wording.

### `Clean Data`
Stores the filtered or validated object set.

### `Errors`
Stores detected issues in row-based form.

### `Summary`
Stores counts, distributions, and high-level project health indicators.

### `By Line`
Stores grouped findings by LineID when useful.

---

## Validation Design Philosophy

The Excel design in this project follows a simple principle:

**Keep the logic explicit, small, and engineering-readable.**

The first version of the project should not try to replicate a full enterprise rule engine.

Instead, the Excel layer should support a compact set of believable checks such as:
- missing IDs;
- missing LineID;
- missing material;
- missing coating for subsea-related items;
- invalid or suspicious wall thickness;
- missing rating where required;
- class or specification inconsistency;
- export completeness mismatches.

This keeps the project realistic and understandable.

---

## Error Reporting Design

The error reporting structure should stay consistent between Python output and Excel presentation.

Recommended error columns:
- ElementID
- ElementType
- FieldName
- IssueType
- IssueDescription
- Severity

This structure makes it easy to:
- review issues manually;
- sort and filter results in Excel;
- summarize issue counts by type or field;
- connect the findings back to specific model objects.

---

## Formatting and Presentation Notes

The final Excel outputs should be readable and presentation-friendly.

Useful formatting practices may include:
- clear worksheet names;
- bold headers;
- frozen top rows;
- filtered tables;
- column widths adjusted for readability;
- severity-based highlighting;
- summary tables with counts by issue type;
- separate sheets for raw data, errors, and summary.

Python tools such as `pandas`, `openpyxl`, and `XlsxWriter` support reading, writing, and formatting Excel outputs, and they are commonly used together in reporting workflows.

---

## Design Boundaries

This Excel layer is intentionally lightweight.

It is not intended to become:
- a full database replacement;
- a complex enterprise dashboard;
- a permanent master data system;
- a substitute for E3D itself.

Its role is to support a compact portfolio workflow:
**E3D model -> export -> validation -> Excel report**

---

## Practical Takeaway

The Excel design is important because it makes the project understandable at three levels:
1. as a modeling workflow;
2. as a validation workflow;
3. as a reporting workflow.

It allows the project to show not only Python automation, but also engineering organization, structured review logic, and clean deliverable packaging.

That is exactly why the Excel layer is a core part of the project rather than a side file collection.
