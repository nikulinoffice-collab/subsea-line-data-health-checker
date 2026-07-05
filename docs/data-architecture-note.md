# Data Architecture Note

## Purpose

This note explains the basic AVEVA E3D data architecture relevant to the **Subsea Line Data Health Checker** project.

The purpose is not to reproduce full AVEVA documentation, but to connect the project workflow to the data structure behind it. The project depends on understanding where model data comes from, how it is organized, and how exported attributes relate to design, catalogue, and specification sources.

---

## Project Context

This portfolio project is based on a compact subsea or pipeline-oriented demo case in AVEVA E3D.

A small line section is modeled in E3D, exported into CSV or Excel format, checked in Python, and reported through Excel-based data quality outputs.

Because of this workflow, it is important to understand:
- where the modeled objects are stored;
- where the object attributes come from;
- how catalogue and specification data influence model content;
- how administration and project setup affect available databases and MDB access.

---

## High-Level Structure

At a high level, an AVEVA E3D project is supported by:
- administration databases;
- model databases;
- MDB configuration;
- project defaults and environment settings.

The administration databases contain the administrative information needed for the project, while the model databases contain actual design and reference data such as model geometry, catalogues, specifications, properties, and dictionary content.

---

## Administration Databases

The administration layer includes the core administrative databases used to control access, communication, and project coordination.

### SYSTEM Database

The SYSTEM database holds the access control data for the project and controls essential project-level administration.

For this project, the SYSTEM database matters because it influences:
- who can access the project;
- which users and teams exist;
- which databases are visible or writable.

### COMMS Database

The COMMS database stores information about who is using which module and which model databases are available.

For this project, COMMS matters because it is part of the project environment that supports coordinated access to model data.

### MISC Database

The MISC database stores inter-user messages and inter-database macros.

This is not the main focus of the portfolio project, but it is part of the standard project architecture and helps explain how the environment is organized.

---

## Model Databases

An E3D project can contain multiple model databases. These are the databases most relevant to the workflow of modeling, export, checking, and reporting.

### Design DB

The Design DB contains the actual design information for the project.

For the **Subsea Line Data Health Checker**, this is the most important source database because the modeled objects come from here.

Examples of design-side content relevant to this project:
- pipe elements;
- elbows;
- flanges;
- valve;
- supports;
- nozzle or tie-in references;
- object attributes assigned during modeling.

When model data is exported into CSV or Excel, the exported rows are mainly derived from the design-side object structure.

### Catalogue DB

The Catalogue DB contains catalogue content used by the design environment.

In practical terms, catalogue data provides the engineering definitions behind components, including geometry, connection logic, and descriptive component information.

For this project, the Catalogue DB matters because fields such as:
- SpecName;
- PipeClass;
- component selection logic;
- geometry consistency;
- connection compatibility

are linked to catalogue and specification decisions rather than only to manual modeling.

### Specification Data

Specifications define which catalogue components are suitable for a given use case.

For this project, specification data matters because it influences:
- which piping components are allowed on the line;
- how class and component choices are controlled;
- whether the exported line data is internally consistent.

In the validation logic, this connection is important when checking:
- missing SpecName;
- inconsistent PipeClass;
- mismatched line-level attributes.

### Properties DB

The Properties DB can contain component and material properties used for engineering purposes such as material behavior, safety review, or weight-related logic.

For this project, the Properties DB is relevant mainly as background context. It helps explain where material-related engineering properties may come from, even if the first version of the project only uses a simplified subset of attributes.

### Dictionary DB

The Dictionary DB contains the definitions of user-defined attributes and related dictionary content.

This matters if the project later introduces custom attributes, naming logic, or user-defined fields in the export and validation workflow.

---

## MDB Role

The MDB, or Multiple Database, is the working database environment through which a user accesses a selected set of databases.

In practice, the MDB determines which databases are available in a particular working context and therefore which project data can be viewed or modified.

For this portfolio project, the MDB matters because:
- the design user must have access to the required design data;
- catalogue and specification databases must be available where needed;
- the export workflow depends on visibility of the correct project content.

A useful practical check in AVEVA Administration is to identify whether the demo project is using a training MDB, a design MDB, or another project-specific MDB with the required database access.

---

## How This Relates to the Project Workflow

The **Subsea Line Data Health Checker** depends on a simple but important data chain:

1. A compact line section is modeled in E3D.
2. The objects are stored in the design-side project data structure.
3. Their engineering meaning is influenced by catalogue and specification sources.
4. The object data is exported into CSV or Excel form.
5. Python scripts validate the exported attributes.
6. Excel reports summarize errors and model data health.

This means the project is not only about geometry. It is about understanding how design objects, attributes, and reference data are connected.

---

## Object-to-Data Mapping

For this project, a modeled object can be understood through three linked layers:

### 1. Design Object Layer
This is the object that exists in the E3D model, for example:
- a pipe segment;
- an elbow;
- a flange;
- a valve;
- a support.

### 2. Attribute Layer
This is the set of fields exported for checking, such as:
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

### 3. Reference Logic Layer
This is the engineering logic behind the object:
- catalogue definition;
- specification selection;
- allowed material or class;
- expected line consistency.

The Python validation logic sits mainly between the exported attribute layer and the reference logic layer.

---

## What Will Be Verified in This Project

This project does not attempt to audit the full AVEVA E3D database architecture.

Instead, it uses a focused subset of the architecture to support several practical checks:
- whether all required objects are exported;
- whether key attributes are present;
- whether line-level identifiers are consistent;
- whether material and coating fields are filled correctly;
- whether class and specification fields are present and believable;
- whether the exported dataset is usable for structured QC reporting.

---

## Practical Takeaway

For this portfolio case, the most important architecture understanding is:

- **Design DB** stores the modeled project objects;
- **Catalogue and Specification data** define what those objects are allowed to be;
- **Administration databases** control project structure and access;
- **MDB configuration** determines which databases the user can work with;
- **CSV/Excel export** is the bridge from E3D to Python-based validation.

This is the key reason why the project demonstrates more than software usage. It shows an understanding of how engineering model data is structured and how that structure can support automation.

---

## Scope Note

This project is intentionally limited in scope.

It does **not** attempt in the first version to include:
- direct E3D API integration;
- full database administration;
- SQL-based storage;
- enterprise rule engines;
- advanced catalogue authoring.

The focus is on a compact and believable workflow:
**E3D model -> export -> validation -> Excel report**
