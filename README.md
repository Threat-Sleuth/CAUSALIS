[ThreatSleuth_Studio_UI_Product_Specification_v0.1.md](https://github.com/user-attachments/files/29840466/ThreatSleuth_Studio_UI_Product_Specification_v0.1.md)
# ThreatSleuth-Studio
Cybersecurity Experiment and Dataset Engineering Platform. Build reproducible cyber experiments. Understand the evidence.
# ThreatSleuth Studio

## UI, Product and Experience Specification

**Document version:** 0.1\
**Target product:** ThreatSleuth Studio\
**Product status:** New private product project\
**Technical baseline:** ThreatSleuth ATG 1.0.0 Stable\
**Document status:** Living specification

------------------------------------------------------------------------

## 1. Product Vision

ThreatSleuth Studio is the provisional name for a new product-oriented
cybersecurity experimentation platform derived from the validated
ThreatSleuth ATG 1.0.0 research tool.

The project is considered a new product initiative rather than a
continuation of the doctoral research software. ThreatSleuth ATG
provides the validated technical baseline, but ThreatSleuth Studio will
be designed for users who did not participate in its development and who
expect a polished, understandable, efficient, and visually coherent
commercial-grade application.

The product vision is:

> **Design, execute, transform, and understand reproducible
> cybersecurity experiments from a single workspace.**

The platform should allow a user to generate controlled normal and
anomalous traffic, correlate execution events with service logs,
construct and assess labeled cybersecurity datasets, train
classification models, analyze experimental results, and, in later
phases, design and execute multi-stage APT scenarios.

The product must hide unnecessary workflow complexity without removing
advanced control.

------------------------------------------------------------------------

## 2. Strategic Principle

The core technical principle remains:

> **DO NOT BREAK WHAT ALREADY WORKS.**

ThreatSleuth ATG 1.0.0 is the frozen technical baseline. Its validated
traffic generation, campaign execution, scheduling, event registration,
log correlation, dataset processing, machine-learning training,
Intelligent Mode, SQLite persistence, and offline deployment
capabilities must be preserved.

ThreatSleuth Studio will evolve around and progressively refactor the
validated core only when a concrete product requirement justifies the
change.

No architectural modernization will be accepted solely for aesthetic or
stylistic reasons.

Every development phase must include regression validation of previously
working functionality.

------------------------------------------------------------------------

## 3. Product Positioning

ThreatSleuth Studio should not be presented primarily as an Automated
Traffic Generator.

The intended product category is broader:

**Cybersecurity Experiment and Dataset Engineering Platform**

The platform combines:

-   controlled traffic generation;
-   reproducible experiment orchestration;
-   log correlation;
-   dataset engineering;
-   data-quality assessment;
-   machine-learning evaluation;
-   integrated experiment analytics;
-   future APT scenario orchestration and ground-truth generation.

The value proposition is not a single dataset or a single traffic
generator. The value lies in providing a repeatable experimental
workflow from controlled activity generation to interpretable results.

------------------------------------------------------------------------

## 4. Target Users

Initial product design should consider four primary user profiles.

### 4.1 Cybersecurity Researcher

Needs reproducible experiments, traceability, dataset generation,
explicit parameters, repeatable campaigns, quality metrics, and
exportable results.

### 4.2 Security Laboratory or R&D Team

Needs a reusable environment for controlled traffic generation, dataset
engineering, model comparison, and validation of security analytics.

### 4.3 University or Training Laboratory

Needs a guided workflow that reduces the technical knowledge required to
execute complete cybersecurity data experiments.

### 4.4 Detection and Security Engineering Professional

Needs controlled generation of observable activity, service-specific
scenarios, experimental evidence, and future ATT&CK-oriented adversary
sequences.

The first release does not need separate editions for these profiles.
The interface should instead use progressive disclosure and
Guided/Advanced interaction modes.

------------------------------------------------------------------------

## 5. Product Experience Principles

The GUI is a core product component, not a visual layer added after
backend development.

The following principles are mandatory:

1.  A new user must understand the main workflow without reading the
    source code or inspecting the filesystem.
2.  The current experiment and its status must always be visible.
3.  The interface must clearly indicate the recommended next action.
4.  Advanced parameters must remain available but should not dominate
    the default workflow.
5.  Results must be understandable directly in the application.
6.  Errors must explain what failed and, whenever possible, what the
    user can do next.
7.  Long-running operations must show visible progress and current
    stage.
8.  Terminology must be consistent across the complete product.
9.  Visual hierarchy must distinguish navigation, primary actions,
    status, warnings, and analytical results.
10. The interface must remain usable at 1024 × 768 and scale
    appropriately to larger displays.
11. Normal operation must not require manual navigation through
    application directories.
12. The product must retain offline operation after installation.

------------------------------------------------------------------------

## 6. Provisional Product Identity

### 6.1 Provisional name

**ThreatSleuth Studio**

The provisional name deliberately preserves the ThreatSleuth lineage
while removing the restrictive ATG designation.

"Studio" reflects the intended evolution from a single-purpose generator
into an integrated environment where users create experiments, generate
traffic, engineer datasets, train models, analyze results, and later
compose APT scenarios.

The name is provisional and must be reviewed before any public
commercial launch. Domain, trademark, company-name, package-name, and
repository-name availability must be checked before final adoption.

### 6.2 Provisional product descriptor

**Cybersecurity Experiment and Dataset Engineering Platform**

### 6.3 Provisional short message

**Build reproducible cyber experiments. Understand the evidence.**

### 6.4 Brand personality

The product identity should communicate:

-   technical credibility;
-   precision;
-   controlled experimentation;
-   traceability;
-   intelligence;
-   modern cybersecurity engineering.

The brand should avoid visual clichés such as hooded attackers, green
Matrix-style code, excessive skull imagery, generic padlocks, and
aggressive "hacker tool" aesthetics.

### 6.5 Visual direction

The initial visual direction should use a professional dark analytical
interface with restrained contrast, strong typography, clear data
visualization, and a distinctive accent system.

The future logo should be simple enough to work as:

-   application mark;
-   browser favicon;
-   GitHub repository identity;
-   report header;
-   product website mark;
-   monochrome icon.

A final logo is outside version 0.1 of this specification.

------------------------------------------------------------------------

## 7. Primary Navigation Model

The current tool-oriented navigation should evolve toward an
experiment-oriented structure.

Proposed primary navigation:

-   **Home**
-   **Experiments**
-   **Traffic**
-   **Data**
-   **Models**
-   **APT Scenarios**
-   **System**

The current experiment should be visible in the application shell.

Example:

**HTTP Validation 2026-07**\
Dataset Ready

A global **Continue Experiment** action should open the next recommended
workflow stage.

Advanced users must retain direct access to targets, profiles,
campaigns, scheduler, processing tools, and model experiments.

------------------------------------------------------------------------

## 8. Home Dashboard

The Home screen should answer three questions immediately:

1.  What am I working on?
2.  What happened recently?
3.  What should I do next?

The screen should contain:

### Active Experiment

-   Experiment name.
-   Current lifecycle state.
-   Services.
-   Last activity.
-   Workflow progress.
-   Recommended next action.

### Recent Experiments

A compact list of recent workspaces with status and last modification
time.

### System Status

-   Application status.
-   Scheduler status.
-   Database status.
-   Storage usage.
-   Active executions.

### Quick Actions

-   New Experiment.
-   Continue Experiment.
-   Discover Environment.
-   Create Campaign.
-   Import Logs.

The Home screen must not reproduce every feature of the application. Its
purpose is orientation and continuation.

------------------------------------------------------------------------

## 9. Experiment Workspace

The Experiment Workspace is the central product concept.

All experiment-related artifacts should be presented in one coherent
context.

Proposed workspace tabs:

-   **Overview**
-   **Environment**
-   **Traffic**
-   **Data**
-   **Models**
-   **Results**
-   **Artifacts**

Future APT-enabled experiments may additionally expose:

-   **Scenario**
-   **Timeline**
-   **Ground Truth**

### 9.1 Overview

The Overview tab displays:

-   experiment purpose;
-   status;
-   workflow progress;
-   configured services;
-   campaign executions;
-   data status;
-   trained models;
-   latest result summary;
-   warnings;
-   next recommended action.

### 9.2 Workflow progress

The standard workflow is:

Create Experiment\
→ Configure Environment\
→ Generate Traffic\
→ Import Logs\
→ Correlate Events\
→ Assess and Clean Data\
→ Build Dataset\
→ Train Models\
→ Analyze Results\
→ Export Report

Each stage must have an explicit state:

-   Not Started.
-   Ready.
-   Running.
-   Completed.
-   Warning.
-   Failed.

Workflow state should be derived from real artifacts whenever possible.

------------------------------------------------------------------------

## 10. Simplified Data Workflow

The current processing functionality is validated but exposes too much
internal workflow complexity to a new user.

ThreatSleuth Studio should preserve the backend processing stages while
presenting a simplified user journey.

### 10.1 Import

The user selects or uploads a log file.

The application should attempt to infer:

-   service;
-   associated experiment;
-   relevant campaign execution;
-   timestamp characteristics;
-   parser compatibility.

Ambiguous information should be requested from the user.

### 10.2 Correlate

The application proposes correlation settings based on the experiment
context.

Default view:

-   Detected service.
-   ATG execution source.
-   Log source.
-   Proposed correlation window.
-   Estimated time range overlap.

Advanced settings may expose:

-   correlation window;
-   matching fields;
-   timezone interpretation;
-   parser options.

### 10.3 Assess

After correlation, the GUI displays a Data Quality Summary:

-   input log records;
-   execution events;
-   matched records;
-   unmatched records;
-   duplicate records;
-   missing values;
-   constant features;
-   near-zero variance features;
-   detected outliers;
-   usable records.

### 10.4 Clean

The application proposes a recommended cleaning configuration.

The user can accept the recommendation or open Advanced Cleaning.

Every applied transformation must be recorded.

### 10.5 Build Dataset

A single primary action builds the dataset from the validated processing
configuration.

The output must retain:

-   source artifacts;
-   service;
-   normal/anomaly context;
-   applied transformations;
-   timestamps;
-   application version;
-   processing parameters.

The user should not need to manually move files between application
directories.

------------------------------------------------------------------------

## 11. Integrated Results Experience

Reports are no longer the primary mechanism for understanding experiment
results.

The GUI becomes the principal analytical surface.

### 11.1 Experiment Results Dashboard

The Results tab should provide an immediate summary:

-   dataset size;
-   class distribution;
-   evaluated service;
-   classification task;
-   number of trained models;
-   best model;
-   Accuracy;
-   Precision;
-   Recall;
-   F1-score;
-   ROC-AUC where applicable;
-   MCC. 

### 11.2 Visual components

The first implementation should support:

-   class distribution chart;
-   CAPEC distribution chart for multiclass experiments;
-   model ranking;
-   metric comparison chart;
-   confusion matrix;
-   experiment timeline;
-   correlation success summary;
-   feature-quality summary.

Each visualization must answer a concrete analytical question.
Decorative charts should not be added.

### 11.3 Best Model Card

The best model should be displayed as a prominent analytical card.

Example:

**Best Model**\
Decision Tree

MCC: 0.997\
F1-score: 0.998\
Accuracy: 0.999

Selection criterion: MCC

The user should be able to open the full model detail view.

### 11.4 Model Ranking

The ranking view should display all valid models and allow sorting by:

-   MCC;
-   F1-score;
-   Accuracy;
-   Precision;
-   Recall;
-   ROC-AUC;
-   training duration.

MCC remains the default ranking criterion.

### 11.5 Confusion Matrix

The confusion matrix must be rendered directly in the GUI.

The visualization should support:

-   raw counts;
-   normalized values;
-   class labels;
-   export as image for report generation.

### 11.6 Dataset Quality View

Dataset quality should have its own analytical view rather than being
presented only during processing.

It should show:

-   record evolution from raw to final dataset;
-   missing-value summary;
-   duplicate summary;
-   entropy or variability indicators;
-   constant and near-zero variance features;
-   outlier summary;
-   selected and removed features.

------------------------------------------------------------------------

## 12. Reporting Strategy

ThreatSleuth Studio will simplify the report model.

### 12.1 Primary report formats

The product will generate:

-   **HTML**
-   **PDF**

HTML is the interactive and portable technical report.

PDF is the fixed-format report intended for sharing, archiving, and
formal documentation.

### 12.2 Technical exports

JSON and CSV may remain available as technical data exports where
useful, but they must not be presented as primary report formats.

### 12.3 Report content

Reports should be generated from the same experiment result model used
by the GUI.

The GUI and reports must not independently calculate metrics.

A report should contain:

-   experiment metadata;
-   dataset summary;
-   processing provenance;
-   model configuration;
-   model ranking;
-   metrics;
-   confusion matrix;
-   relevant charts;
-   software version;
-   random seed where applicable;
-   execution timestamp.

### 12.4 Integrated report viewer

HTML reports should be viewable directly from the application.

The user should be able to:

-   View Report.
-   Export HTML.
-   Export PDF.

Report generation becomes an export capability, not the main results
workflow.

------------------------------------------------------------------------

## 13. Visual and Interaction Requirements

The interface should evolve from the current functional research UI
toward a coherent product design system.

The design system should define:

-   typography scale;
-   spacing system;
-   card styles;
-   button hierarchy;
-   form controls;
-   status badges;
-   progress indicators;
-   tables;
-   charts;
-   modal behavior;
-   empty states;
-   loading states;
-   warning states;
-   error states.

Primary actions must be visually unambiguous.

Destructive actions must be clearly distinguished and require
appropriate confirmation.

Long-running tasks such as scanning, campaign execution, correlation,
cleaning, and model training must expose progress.

The GUI should avoid dense walls of controls. Advanced options should
use progressive disclosure.

------------------------------------------------------------------------

## 14. Branding Preparation Requirements

Branding work will be developed in a later dedicated phase, but the
product architecture must already support it.

The application should centralize:

-   product name;
-   product descriptor;
-   version;
-   logo asset;
-   favicon;
-   accent configuration;
-   report branding;
-   application title.

The product name must not be hard-coded throughout backend and frontend
files.

Reports must consume shared product metadata.

This requirement allows the provisional ThreatSleuth Studio identity to
be replaced without invasive code changes if a final commercial name is
selected.

------------------------------------------------------------------------

## 15. Private Repository Strategy

ThreatSleuth Studio should be developed in a new private GitHub
repository.

ThreatSleuth ATG 1.0.0 remains the stable research baseline.

Recommended initial repository structure:

``` text
threatsleuth-studio/
├── app/
├── data/
├── docs/
│   ├── architecture/
│   ├── product/
│   └── validation/
├── migrations/
├── tests/
├── scripts/
├── README.md
├── VERSION
└── CHANGELOG.md
```

The initial baseline import should be tagged:

`atg-v1.0.0-baseline`

The first product-development branch should focus on the new application
shell and Experiment Workspace.

No APT functionality should be introduced in the first implementation
phase.

------------------------------------------------------------------------

## 16. Development Phases

### Phase 0 --- Baseline and Product Foundation

-   Verify the exact ThreatSleuth ATG 1.0.0 stable package.
-   Import the baseline into the private repository.
-   Record package hash.
-   Document current modules, endpoints, SQLite schema, and validated
    workflows.
-   Define regression checklist.
-   Centralize product metadata.
-   Establish versioning and migration strategy.

### Phase 1 --- Product Shell and Visual Redesign

-   Introduce the new application shell.
-   Implement primary navigation.
-   Implement Home Dashboard.
-   Establish the initial design system.
-   Improve forms, status feedback, progress indicators, and error
    presentation.
-   Preserve all existing backend functionality.

### Phase 2 --- Experiment Workspace

-   Add Experiment entity.
-   Associate existing artifacts with experiments.
-   Implement experiment lifecycle.
-   Implement Overview, Environment, Traffic, Data, Models, Results, and
    Artifacts views.
-   Add Continue Experiment behavior.

### Phase 3 --- Simplified Dataset Workflow

-   Guided log import.
-   Service detection proposal.
-   Correlation proposal.
-   Integrated data-quality summary.
-   Recommended cleaning configuration.
-   One-action dataset build.
-   Processing provenance.

### Phase 4 --- Integrated Results Dashboard

-   Experiment result summary.
-   Best Model Card.
-   Model ranking.
-   Metric comparison.
-   Confusion matrix.
-   Class distribution.
-   Dataset quality visualization.
-   Integrated report viewer.

### Phase 5 --- Reporting Simplification

-   Consolidate report generation.
-   HTML report.
-   PDF report.
-   Technical data exports separated from reports.
-   Shared result model for GUI and reports.

### Phase 6 --- APT Scenario Engine

-   APT scenario and stage entities.
-   ATT&CK metadata.
-   Stage sequencing.
-   Shared execution context.
-   Ground-truth generation.
-   Scenario visualization.

### Phase 7 --- Product Branding and Market Preparation

-   Final product naming study.
-   Trademark and domain checks.
-   Final logo.
-   Visual identity.
-   Product messaging.
-   User personas validation.
-   Commercial positioning.
-   Product website concept.
-   Packaging and licensing strategy.

------------------------------------------------------------------------

## 17. Phase 1 Success Criteria

The first product-oriented milestone will be considered successful when:

1.  All validated ATG 1.0.0 workflows continue to operate.
2.  The application has a coherent modern visual shell.
3.  Navigation is understandable to a user unfamiliar with the original
    project.
4.  The current experiment concept is visible in the GUI, even if the
    full workspace lifecycle is not yet complete.
5.  Long-running operations provide clear progress feedback.
6.  ML experiment results can begin to be consumed from an integrated
    results area.
7.  Product identity is centralized and can be changed without invasive
    refactoring.
8.  Regression validation confirms that traffic generation, scheduling,
    processing, dataset construction, and model training remain
    functional.

------------------------------------------------------------------------

## 18. Immediate Next Step

Before implementing the visual redesign, the exact ThreatSleuth ATG
1.0.0 stable package must be analyzed and documented as the immutable
baseline.

The first engineering artifact after this specification will be:

**ThreatSleuth_Studio_Baseline_Architecture_v0.1.md**

It will document:

-   package structure;
-   Python modules;
-   principal functions;
-   API endpoints;
-   frontend structure;
-   SQLite tables and relationships;
-   filesystem persistence;
-   validated functional workflows;
-   technical dependencies;
-   regression-critical behavior.

Only after this baseline document is complete should Phase 1
implementation begin.
