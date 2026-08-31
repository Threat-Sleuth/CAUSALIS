# CAUSALIS

```{=html}
<p align="center">
```
`<img src="app/static/brand/causalis_logo_primary.png" alt="CAUSALIS" width="720">`{=html}
```{=html}
</p>
```
```{=html}
<p align="center">
```
`<strong>`{=html}Controlled Cyber
Experimentation`</strong>`{=html}`<br>`{=html} `<em>`{=html}From
controlled action to measurable evidence.`</em>`{=html}
```{=html}
</p>
```
```{=html}
<p align="center">
```
`<strong>`{=html}Version 0.9.1 · Research Stable`</strong>`{=html}
```{=html}
</p>
```

------------------------------------------------------------------------

## Overview

**CAUSALIS** is an integrated platform for **controlled, reproducible
cybersecurity experimentation**. It supports the complete experimental
lifecycle, from the definition of targets and traffic profiles to
campaign execution, evidence collection, dataset engineering,
machine-learning evaluation, result analysis, and scientific reporting.

The platform was created to solve a recurring problem in experimental
cybersecurity research: the difficulty of moving from a controlled cyber
action to a dataset whose origin, labels, transformations, and
analytical results remain traceable and reproducible.

CAUSALIS therefore treats experimentation as a continuous evidence
pipeline rather than as a collection of unrelated tools.

``` text
Experimental environment
        ↓
Targets and traffic profiles
        ↓
Controlled campaign execution
        ↓
Normal and anomalous interactions
        ↓
Execution evidence and service logs
        ↓
Temporal correlation and ground truth
        ↓
Dataset quality assessment
        ↓
Cleaning and dataset construction
        ↓
Machine-learning experiments
        ↓
Metrics, visual analytics and reports
```

CAUSALIS evolved from the validated **ThreatSleuth ATG** research
baseline. The original traffic-generation, scheduling,
event-correlation, dataset-processing, and machine-learning workflows
were progressively integrated into a product-oriented research platform
while preserving compatibility with the validated core.

The governing engineering principle throughout this evolution has been:

> **DO NOT BREAK WHAT ALREADY WORKS.**

Version **0.9.1** is the current **tested and stable research
baseline**.

------------------------------------------------------------------------

## Why CAUSALIS Exists

Cybersecurity datasets are often consumed as finished artifacts.
Researchers receive a CSV file, select features, train models, and
compare metrics. However, the experimental process that produced the
data may be partially documented, difficult to reproduce, or
disconnected from the final labels.

CAUSALIS approaches the problem from the opposite direction.

Instead of starting with a dataset, it starts with the **experiment**.

The platform records the controlled actions that are executed,
associates them with targets and traffic profiles, preserves execution
timestamps and attack metadata, correlates those events with the
observations produced by monitored services, and uses that evidence to
construct labelled datasets.

This makes the experimental chain itself a first-class research object.

The objective is not merely to generate more cybersecurity data, but to
generate data for which the researcher can answer questions such as:

-   What experiment produced this record?
-   Which service was involved?
-   Was the activity normal or anomalous?
-   Which controlled action generated it?
-   When was the action executed?
-   Which execution evidence supports the label?
-   Which CAPEC category is associated with the anomalous activity?
-   Which transformations were applied before training?
-   Which dataset was used by a machine-learning experiment?
-   Which model produced a given result?
-   Which metrics and confusion matrix belong to that experiment?

This traceability is central to the CAUSALIS research philosophy.

------------------------------------------------------------------------

## Research Objectives

CAUSALIS is designed to support research in:

-   reproducible cybersecurity experimentation;
-   controlled generation of normal and anomalous traffic;
-   honeynet-based experimentation;
-   cybersecurity dataset engineering;
-   experimental ground-truth construction;
-   binary anomaly detection;
-   attack classification and attribution;
-   CAPEC-based multiclass classification;
-   comparative machine-learning evaluation;
-   experimental provenance and traceability;
-   security education and laboratory exercises;
-   future multi-stage and APT-oriented experimentation.

The platform is especially useful when the researcher controls both the
traffic-generation environment and the systems that produce the
corresponding telemetry.

------------------------------------------------------------------------

## Core Design Principles

### 1. Experiment first

The experiment is the central concept. Traffic, logs, datasets, models,
metrics, and reports are different stages of the same experimental
process.

### 2. Controlled execution

Traffic generation must take place against explicitly authorized
laboratory targets. CAUSALIS is not designed as an offensive framework
for uncontrolled Internet use.

### 3. Evidence before labels

Labels should be supported by execution evidence whenever possible.
CAUSALIS records its own actions and correlates them with observed
service events.

### 4. Reproducibility

Important experimental parameters are made explicit and persisted so
that an experiment can be understood and, where possible, repeated.

### 5. Traceability

The platform preserves the relationship between experimental
configuration, execution events, generated datasets, machine-learning
experiments, and reports.

### 6. Incremental evolution

Validated functionality is preserved while new capabilities are
introduced. Large refactors are avoided unless they provide a clear
benefit and can be covered by regression validation.

### 7. Research usability

CAUSALIS combines an accessible graphical workflow with enough control
for expert experimentation. Advisory checks guide the user without
unnecessarily blocking legitimate research workflows.

------------------------------------------------------------------------

# Main Capabilities

## Controlled Traffic Generation

CAUSALIS can generate controlled interactions against configured
laboratory services.

Traffic may represent:

-   **normal activity**, intended to reproduce legitimate service usage;
    or
-   **anomalous activity**, intended to reproduce controlled
    security-relevant behaviour.

The platform associates traffic execution with persistent metadata so
that generated actions can later be correlated with service logs.

The execution engine supports:

-   one-off actions;
-   reusable traffic profiles;
-   multi-action campaigns;
-   normal and anomalous modes;
-   service-specific execution;
-   duration control;
-   rate control;
-   concurrency control;
-   immediate execution;
-   scheduled execution;
-   persistent execution events.

The validated execution concept is:

``` text
Campaign / Campaign Group
        ↓
Execution loop
        ↓
Workers
        ↓
Service-specific interaction
        ↓
Persistent execution evidence
```

Each real interaction is coupled with an execution record rather than
being treated as an unobservable background action.

------------------------------------------------------------------------

## Targets

A **target** represents an authorized service or endpoint used in an
experiment.

Targets allow CAUSALIS to separate the definition of the experimental
infrastructure from the behaviour that will be executed against it.

Depending on the service, target configuration can include information
such as:

-   host or address;
-   service;
-   port;
-   protocol-specific parameters;
-   authentication information required by the laboratory scenario;
-   descriptive metadata.

Targets can be created manually and reused across experiments.

------------------------------------------------------------------------

## Traffic Profiles

A **profile** describes how CAUSALIS interacts with a target.

Profiles separate behaviour from infrastructure, allowing the same
target to participate in different experimental scenarios.

Profiles may represent normal or anomalous behaviour and can contain
service-specific parameters required by the corresponding interaction.

This separation supports reusable experimental design:

``` text
Target = where the experiment acts
Profile = how the experiment acts
Campaign = when and how often the action is executed
```

------------------------------------------------------------------------

## Campaigns

Campaigns combine targets, profiles, execution parameters, and timing
into repeatable experimental units.

They support:

-   multiple actions;
-   immediate execution;
-   scheduled execution;
-   rate configuration;
-   concurrency configuration;
-   duration control;
-   normal/anomalous experimental balance;
-   persistent campaign history.

Campaign design makes it possible to generate controlled volumes of
activity while retaining knowledge of the actions that produced the
resulting telemetry.

### Rate and concurrency

CAUSALIS distinguishes between the rate at which interactions are
generated and the number of concurrent workers involved in execution.

These parameters allow researchers to adjust experimental intensity
without modifying the semantic definition of the traffic profile.

Because they affect both workload and temporal correlation, rate and
concurrency are considered part of the experimental configuration rather
than simple performance settings.

------------------------------------------------------------------------

## Scheduler

CAUSALIS includes scheduling capabilities for campaigns that must
execute at a future time.

Scheduling is part of the validated core and is treated as
regression-critical because experimental reproducibility depends on
correct handling of execution time.

The platform preserves the semantics of scheduled campaigns and their
associated execution evidence.

------------------------------------------------------------------------

## Intelligent Mode

**Intelligent Mode** assists the researcher in preparing experiments in
an authorized environment.

Its purpose is to reduce manual configuration by helping identify
available services and proposing suitable experimental elements.

The workflow follows a **review-before-apply** model:

``` text
Authorized environment
        ↓
Discovery
        ↓
Service identification
        ↓
Proposed configuration
        ↓
Researcher review
        ↓
Explicit application
```

Intelligent Mode is intended for private, controlled, or expressly
authorized networks.

It is not a mechanism for indiscriminate Internet scanning.

------------------------------------------------------------------------

# Experimental Evidence and Ground Truth

## Execution Events

CAUSALIS records execution events produced by controlled traffic
generation.

These events form the primary evidence that the platform itself
generated a particular interaction.

Execution records may include contextual information such as:

-   timestamp;
-   service;
-   target;
-   traffic mode;
-   campaign context;
-   execution outcome;
-   attack-related metadata where applicable.

This information is later used during log correlation.

------------------------------------------------------------------------

## Service Logs

The experimental services independently produce their own telemetry.

CAUSALIS can ingest service logs and process them as observations of the
experiment.

The key distinction is:

``` text
CAUSALIS execution event
        =
What the experiment says it executed

Service log
        =
What the monitored service says it observed
```

The correlation stage connects these two perspectives.

------------------------------------------------------------------------

## Temporal Correlation

Execution evidence and service observations are correlated using
temporal information and experimental context.

The purpose is to identify which service records can reasonably be
associated with controlled CAUSALIS actions.

The resulting matched records provide the basis for labelled
experimental datasets.

Temporal correlation is particularly important because service logging
and traffic execution do not necessarily share identical timestamps.
Processing, buffering, network latency, and logging behaviour may
introduce small differences.

CAUSALIS therefore treats the correlation window as an experimental
parameter rather than assuming exact timestamp equality.

------------------------------------------------------------------------

## Ground Truth

In CAUSALIS, **ground truth** refers to the experimentally supported
knowledge used to determine the class associated with an observation.

Because the platform controls the actions that generate the traffic, it
can preserve information about whether an action was intended as normal
or anomalous and, for supported attack experiments, the corresponding
attack category.

This does not mean that every service event is automatically labelled
solely because it occurred near an experiment. Correlation quality
remains important.

The objective is to make the origin of labels explicit and auditable.

------------------------------------------------------------------------

# Data Engineering

CAUSALIS includes a data-engineering workflow designed specifically for
experimental cybersecurity datasets.

The objective is to move from correlated raw observations to reusable
machine-learning datasets without hiding important data-quality
decisions.

## Dataset Quality Analysis

The quality pipeline can assess characteristics including:

-   missing values;
-   duplicate records;
-   inferred data types;
-   text quality;
-   variability;
-   entropy;
-   domain consistency;
-   potential outliers;
-   class distribution;
-   other service-specific quality indicators.

The researcher can inspect the dataset before applying cleaning
operations.

------------------------------------------------------------------------

## Cleaning

Cleaning is configurable rather than silently imposed.

The platform is designed to make the researcher aware of the
transformations applied to the data.

Depending on the dataset and selected workflow, cleaning can address
issues such as:

-   missing values;
-   duplicate records;
-   unsuitable columns;
-   malformed values;
-   low-information fields;
-   inconsistent data representations.

Generated cleaned datasets are stored as separate artifacts so that raw
inputs are not silently overwritten.

------------------------------------------------------------------------

## Dataset Construction

CAUSALIS can transform correlated and cleaned observations into datasets
suitable for machine-learning experiments.

The platform supports:

-   labelled binary datasets;
-   CAPEC-oriented multiclass datasets;
-   train/test split generation;
-   reusable exported datasets;
-   multi-service research workflows.

The distinction between raw logs, matched logs, cleaned data, generated
datasets, and train/test artifacts is preserved.

------------------------------------------------------------------------

## Train/Test Splits

Train/test datasets can be generated from prepared experimental data.

A common experimental configuration uses an **80/20 split**, although
the appropriate split remains a methodological decision for the
researcher.

CAUSALIS stores generated split artifacts so that model evaluation can
be associated with the actual data used by the experiment.

------------------------------------------------------------------------

# Machine Learning

CAUSALIS integrates machine-learning evaluation into the same
experimental environment used to produce and engineer the data.

The objective is not to replace general-purpose ML frameworks. Instead,
CAUSALIS provides a reproducible evaluation layer connected to the
provenance of the cybersecurity experiment.

The implementation uses **scikit-learn** classifiers through shared
training workflows.

------------------------------------------------------------------------

## Binary Classification

Binary experiments distinguish between:

``` text
Normal
Attack
```

This mode is intended for anomaly or attack detection where the primary
research question is whether an observation belongs to normal or
anomalous activity.

Binary experiments can be performed on service-specific datasets.

------------------------------------------------------------------------

## Multi-Service Binary Evaluation

CAUSALIS supports sequential evaluation across multiple service
datasets.

This allows researchers to compare how the same general detection
methodology behaves across heterogeneous services without manually
repeating the complete workflow.

The interface includes advisory consistency checks to help detect
accidental mismatches between the selected service and dataset filename.

These checks are intentionally non-blocking because advanced experiments
may legitimately use heterogeneous data.

------------------------------------------------------------------------

## CAPEC Multiclass Classification

CAUSALIS also supports multiclass experiments based on **CAPEC** attack
categories.

In this mode, the objective is not only to detect anomalous activity but
to distinguish among supported attack classes.

Conceptually:

``` text
Observed event
      ↓
Attack detection / labelled attack data
      ↓
CAPEC-oriented class attribution
```

This enables research beyond simple binary detection and supports
experiments in attack characterization and attribution.

------------------------------------------------------------------------

## Evaluation Metrics

CAUSALIS persists and visualizes multiple metrics because no single
score adequately characterizes classifier performance.

Depending on experiment type and model capabilities, the platform
includes metrics such as:

-   **Accuracy**
-   **Precision**
-   **Recall**
-   **F1-score**
-   **Balanced Accuracy**
-   **Matthews Correlation Coefficient (MCC)**
-   **ROC-AUC**

### Why MCC matters

MCC is particularly useful in cybersecurity experiments because datasets
may be imbalanced. It incorporates all four elements of the binary
confusion matrix and provides a more informative view than accuracy
alone when one class dominates.

CAUSALIS therefore uses MCC prominently in model ranking and comparative
visualization.

------------------------------------------------------------------------

## Confusion Matrices

Confusion matrices are stored as part of experiment results and
visualized in the interface.

They provide a direct view of classification behaviour that aggregate
metrics cannot fully express.

For binary experiments, they expose:

-   true positives;
-   true negatives;
-   false positives;
-   false negatives.

For multiclass experiments, they show how predictions are distributed
across the supported classes.

------------------------------------------------------------------------

## Experiment Persistence

Machine-learning experiments are persisted rather than treated as
temporary browser sessions.

Stored information includes experiment metadata and model results,
enabling the user to return to previous experiments and generate reports
without retraining the models.

This is an important reproducibility feature:

> **Viewing a result or generating a report does not retrain the
> experiment.**

Reports and dashboards consume persisted results.

------------------------------------------------------------------------

# Analytics and Results

CAUSALIS provides integrated result exploration directly in the
graphical interface.

Depending on the selected experiment, the Results area can present:

-   experiment metadata;
-   best-performing model;
-   core metrics;
-   model ranking;
-   MCC comparison;
-   metric profiles;
-   confusion matrices;
-   class distribution;
-   training-time versus performance relationships;
-   detailed metric tables;
-   experiment history.

The purpose is to provide immediate experimental interpretation without
requiring the researcher to export every result to a separate analysis
environment.

------------------------------------------------------------------------

# Reporting

CAUSALIS can generate research-oriented reports from persisted
experiment results.

Primary user-facing formats are:

-   **HTML**
-   **PDF**
-   **HTML + PDF**

When both formats are requested, they can be packaged together for
convenient distribution or archival.

## HTML Reports

HTML reports are designed to be portable and browser-ready.

They can include:

-   CAUSALIS branding;
-   experiment metadata;
-   model metrics;
-   rankings;
-   confusion matrices;
-   dataset information;
-   configuration and provenance information.

## PDF Reports

PDF reports provide a fixed-layout artifact suitable for:

-   research records;
-   sharing;
-   archival;
-   inclusion in experimental evidence;
-   supporting documentation.

PDF generation uses **ReportLab**.

## Technical Exports

CSV and JSON artifacts, where produced by a workflow, are considered
technical data exports rather than primary human-readable reports.

------------------------------------------------------------------------

# User Interface

CAUSALIS is delivered as a browser-based research application.

The current application combines a FastAPI backend with a lightweight
frontend based on HTML, CSS, and vanilla JavaScript.

The interface organizes the experimental workflow into functional areas
such as:

-   Home;
-   experiment configuration;
-   traffic and campaigns;
-   Intelligent Mode;
-   data engineering;
-   machine learning;
-   results and analytics;
-   reporting.

The graphical interface is intended to expose the complete research
workflow without hiding the underlying experimental concepts.

Version 0.9.1 represents the current stable interface baseline. Future
visual evolution will be performed independently from the validated
functional core.

------------------------------------------------------------------------

# Home Dashboard

The Home area provides an operational overview of the CAUSALIS
environment.

It can summarize information such as:

-   configured targets;
-   available profiles;
-   campaign activity;
-   scheduler state;
-   recorded execution events;
-   ML experiment counts;
-   recent experiments;
-   recent reports;
-   experiment readiness;
-   service distribution;
-   normal/anomalous event distribution;
-   model summaries.

The dashboard is informational. It does not recalculate model results.

------------------------------------------------------------------------

# Architecture

CAUSALIS follows a local web-application architecture.

``` text
Browser
   │
   │ HTML / CSS / JavaScript / JSON
   ▼
FastAPI Application
   │
   ├── Product Interface
   ├── Target and Profile Management
   ├── Campaign Orchestration
   ├── Traffic Generation Engine
   ├── Scheduler
   ├── Intelligent Mode
   ├── Execution Event Registration
   ├── Log Ingestion
   ├── Temporal Correlation
   ├── Dataset Quality Analysis
   ├── Dataset Cleaning
   ├── Dataset Construction
   ├── Machine-Learning Orchestration
   ├── Analytics
   └── HTML/PDF Reporting
   │
   ├──────────────► SQLite
   │
   └──────────────► Filesystem Artifacts
```

The application is served by **Uvicorn**.

No separate JavaScript frontend framework is required.

------------------------------------------------------------------------

# Technology Stack

The validated CAUSALIS stack includes:

-   **Python**
-   **FastAPI**
-   **Uvicorn**
-   **SQLite**
-   **scikit-learn**
-   **ReportLab**
-   **Vanilla JavaScript**
-   **HTML**
-   **CSS**
-   **Bash deployment scripts**

The architecture deliberately remains relatively lightweight so that
CAUSALIS can be deployed as a self-contained research tool in laboratory
environments.

------------------------------------------------------------------------

# Persistence

CAUSALIS uses a hybrid persistence model based on **SQLite** and
filesystem artifacts.

## SQLite

The historical database filename is preserved for compatibility:

``` text
data/atg.db
```

The validated database stores information such as:

-   targets;
-   profiles;
-   campaign groups;
-   execution events;
-   application settings;
-   machine-learning experiments;
-   machine-learning results.

The legacy filename is intentionally retained even though the product is
now CAUSALIS.

## Filesystem Artifacts

The filesystem stores artifacts such as:

-   uploaded service logs;
-   matched/correlated logs;
-   cleaned datasets;
-   train/test splits;
-   ML-ready datasets;
-   trained model artifacts;
-   generated reports;
-   configuration exports.

A typical data structure is:

``` text
data/
├── atg.db
├── config/
├── exports/
│   ├── matched_logs/
│   ├── clean_datasets/
│   ├── dataset_splits/
│   ├── ml_datasets/
│   └── ml_reports/
├── logs/
├── models/
└── uploads/
```

------------------------------------------------------------------------

# Repository Structure

A typical CAUSALIS source tree is organized as follows:

``` text
CAUSALIS/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── static/
│   │   ├── app.js
│   │   ├── style.css
│   │   └── brand/
│   └── templates/
│       └── index.html
├── data/
│   ├── atg.db
│   ├── config/
│   ├── exports/
│   ├── logs/
│   ├── models/
│   └── uploads/
├── docs/
├── deploy.sh
├── run.sh
├── stop.sh
├── requirements.txt
├── VERSION
├── CHANGELOG.md
└── README.md
```

The exact package contents may evolve while compatibility-critical paths
remain stable.

------------------------------------------------------------------------

# Installation

CAUSALIS is primarily developed and validated for Linux research
environments and has been tested on **Kali Linux**.

A Debian-compatible Linux distribution is recommended.

## Requirements

-   Python 3;
-   a Debian-compatible Linux environment;
-   Internet access during initial dependency installation;
-   network access to an explicitly authorized laboratory environment.

## Deploy

From the CAUSALIS directory:

``` bash
chmod +x deploy.sh run.sh stop.sh
./deploy.sh
```

The deployment process prepares the local environment and installs the
required dependencies.

After installation, the application is designed to operate locally
without depending on external cloud services for its core workflows.

------------------------------------------------------------------------

# Running CAUSALIS

Start the application with:

``` bash
./run.sh
```

The default deployment exposes the application through its local web
server.

A typical address is:

``` text
http://<HOST_IP>:8080
```

The port can be overridden with:

``` bash
CAUSALIS_PORT=8081 ./run.sh
```

For backward compatibility with the original research baseline, the
legacy `ATG_PORT` environment variable may also be accepted.

Stop CAUSALIS with:

``` bash
./stop.sh
```

------------------------------------------------------------------------

# Recommended Experimental Workflow

## 1. Prepare the laboratory

Deploy or identify the authorized systems that will participate in the
experiment.

## 2. Configure targets

Register the services against which CAUSALIS will generate controlled
activity.

## 3. Configure traffic profiles

Define the normal and anomalous behaviours required by the experiment.

## 4. Build a campaign

Combine targets and profiles and define rate, concurrency, duration, and
scheduling.

## 5. Execute the experiment

Run the campaign and allow CAUSALIS to persist execution evidence.

## 6. Collect service telemetry

Obtain the logs generated independently by the experimental services.

## 7. Import and correlate

Upload service logs and correlate them with CAUSALIS execution events.

## 8. Assess data quality

Inspect missing data, duplicates, variability, outliers, class balance,
and other quality indicators.

## 9. Clean the dataset

Apply explicit cleaning decisions and generate a separate cleaned
artifact.

## 10. Build ML datasets

Generate labelled datasets and, where required, train/test splits.

## 11. Train models

Run binary, multi-service, or CAPEC multiclass experiments.

## 12. Compare results

Review metrics, rankings, confusion matrices, and class behaviour.

## 13. Generate reports

Produce HTML, PDF, or combined experiment reports.

## 14. Preserve evidence

Archive the relevant database, datasets, reports, and experimental
configuration required to reproduce or audit the work.

------------------------------------------------------------------------

# Backup and Database Compatibility

Before replacing, upgrading, or restoring a CAUSALIS installation, stop
the application.

Example backup:

``` bash
./stop.sh
cp data/atg.db ~/causalis_atg.db.backup
```

Example restoration:

``` bash
./stop.sh
cp /path/to/backup/atg.db data/atg.db
./run.sh
```

Do not replace the SQLite database while CAUSALIS is running.

Compatibility with validated experimental data is considered a
regression-critical requirement.

------------------------------------------------------------------------

# Reproducibility

CAUSALIS is designed around the idea that reproducibility requires more
than preserving source code.

A reproducible cybersecurity experiment may require preservation of:

-   software version;
-   target configuration;
-   traffic profiles;
-   campaign parameters;
-   execution times;
-   rate and concurrency;
-   generated execution events;
-   service logs;
-   correlation parameters;
-   cleaning decisions;
-   dataset versions;
-   train/test splits;
-   model configuration;
-   evaluation metrics;
-   generated reports.

CAUSALIS brings these elements into a common workflow so that the
relationship between them is easier to preserve and inspect.

------------------------------------------------------------------------

# Experimental Provenance

A central long-term objective of CAUSALIS is to maintain a clear chain
from experimental action to analytical conclusion.

Conceptually:

``` text
Research question
      ↓
Experiment configuration
      ↓
Controlled action
      ↓
Execution evidence
      ↓
Observed telemetry
      ↓
Correlation
      ↓
Labelled record
      ↓
Dataset
      ↓
Model experiment
      ↓
Metric / confusion matrix
      ↓
Report
```

This provenance-oriented perspective is particularly important for
research in which datasets and machine-learning results must be
defensible, explainable, and reproducible.

------------------------------------------------------------------------

# Integration with Experimental Honeynets

CAUSALIS is designed to operate naturally with controlled honeynet
environments.

A honeynet provides realistic services and independently generated
telemetry. CAUSALIS provides the orchestration and experimental layer
that generates controlled normal and anomalous activity and records what
was executed.

The combination enables an experimental model in which:

``` text
CAUSALIS
   │
   ├── generates controlled normal traffic
   ├── generates controlled anomalous traffic
   └── records execution ground truth
            │
            ▼
       HONEynet services
            │
            └── generate independent service logs
                         │
                         ▼
                    Correlation
                         │
                         ▼
                    Datasets
```

This separation between traffic generation and service observation is
useful because the resulting dataset is based on telemetry produced by
the monitored systems rather than on synthetic rows created directly by
the ML pipeline.

------------------------------------------------------------------------

# Scientific Use Cases

CAUSALIS can support scenarios such as:

### Controlled dataset generation

Generate repeatable normal and anomalous traffic against laboratory
services and transform the resulting telemetry into labelled datasets.

### Detection-model evaluation

Compare several machine-learning algorithms on datasets produced under
known experimental conditions.

### Service comparison

Evaluate whether the same detection approach behaves consistently across
different protocols or services.

### Attack attribution

Use CAPEC-oriented labels to evaluate multiclass classification beyond
binary attack detection.

### Reproducibility studies

Repeat campaigns under comparable parameters and study the stability of
datasets or model results.

### Teaching

Demonstrate the complete path from cyber activity to telemetry, labelled
data, classification, and evaluation.

### Future APT experimentation

Extend controlled experimentation toward multi-stage scenarios in which
actions form sequences rather than isolated events.

------------------------------------------------------------------------

# Responsible Use

CAUSALIS is intended exclusively for legitimate and authorized purposes,
including:

-   cybersecurity research;
-   university teaching;
-   controlled laboratory experimentation;
-   dataset generation;
-   defensive model evaluation;
-   validation of security research methods.

**Do not use CAUSALIS against systems, services, or networks without
explicit authorization.**

The operator is responsible for:

-   obtaining appropriate authorization;
-   isolating the experimental environment when necessary;
-   protecting credentials and collected data;
-   controlling traffic intensity;
-   complying with applicable law and institutional policies;
-   ensuring that Intelligent Mode is restricted to authorized address
    ranges.

CAUSALIS is a research experimentation platform, not a general-purpose
offensive exploitation framework.

------------------------------------------------------------------------

# Version 0.9.1 --- Research Stable

**CAUSALIS v0.9.1** is the current tested and stable baseline.

This designation means that the release is the reference point for
future development and research use until explicitly superseded by a
newer validated version.

The release consolidates the evolution of CAUSALIS from its original
research core into an integrated experimental platform with:

-   controlled traffic orchestration;
-   reusable targets and profiles;
-   campaign management;
-   scheduling;
-   persistent execution evidence;
-   Intelligent Mode;
-   log ingestion and correlation;
-   dataset quality analysis;
-   configurable cleaning;
-   dataset construction;
-   train/test generation;
-   binary machine-learning experiments;
-   multi-service evaluation;
-   CAPEC multiclass experiments;
-   persisted experiment history;
-   integrated analytics;
-   confusion matrices;
-   comparative metrics;
-   HTML reporting;
-   PDF reporting;
-   combined report packaging;
-   SQLite persistence;
-   branded graphical interface;
-   compatibility with the validated research workflow.

The importance of v0.9.1 is therefore not limited to the changes
introduced by that particular version. It represents the accumulated,
tested state of the complete CAUSALIS research workflow.

------------------------------------------------------------------------

# Stability Policy

Version 0.9.1 is treated as a **frozen functional baseline**.

Future work should follow these rules:

1.  Preserve existing validated functionality.
2.  Avoid changes to experimental semantics without explicit
    justification.
3.  Maintain compatibility with existing research artifacts whenever
    feasible.
4.  Separate visual refactoring from functional changes.
5.  Validate regression-critical workflows before declaring a new stable
    release.
6.  Preserve reproducibility and provenance as primary design
    constraints.

------------------------------------------------------------------------

# Regression-Critical Validation

A stable CAUSALIS release should verify, at minimum:

1.  deployment and startup;
2.  database compatibility;
3.  target persistence;
4.  profile persistence;
5.  one-off traffic execution;
6.  campaign execution;
7.  scheduled execution;
8.  rate and concurrency behaviour;
9.  execution-event persistence;
10. Intelligent Mode;
11. log upload;
12. temporal matching/correlation;
13. dataset quality analysis;
14. cleaning;
15. dataset generation;
16. train/test split generation;
17. binary training;
18. multi-service training;
19. CAPEC multiclass training;
20. experiment persistence;
21. Results visualization;
22. confusion-matrix visualization;
23. HTML report generation;
24. PDF report generation;
25. combined report generation;
26. preservation of validated core behaviour.

------------------------------------------------------------------------

# Current Development Status

CAUSALIS v0.9.1 is currently **feature-frozen as the stable research
baseline** while the associated doctoral research is being documented.

This pause is intentional.

It prevents unnecessary changes to the software while experimental
results, figures, methodology, and reproducibility evidence are being
consolidated in the doctoral thesis.

Development will resume from v0.9.1 rather than from an experimental
branch.

------------------------------------------------------------------------

# Roadmap

The roadmap is intentionally divided between visual consolidation and
future research capabilities.

## 1. CAUSALIS Design System

Before major new interface development, CAUSALIS will define a coherent
visual system covering:

-   official color palette;
-   typography hierarchy;
-   spacing grid;
-   border radii;
-   elevation and shadows;
-   buttons;
-   forms;
-   cards and panels;
-   tables;
-   progress indicators;
-   semantic status colors;
-   iconography;
-   navigation;
-   accessibility;
-   consistent experiment-execution layouts.

The objective is to make future modules visually coherent without
modifying the stable experimental core.

## 2. Visual Refactoring

Existing screens will progressively adopt the Design System while
preserving functionality.

A major goal is to eliminate unnecessary nested containers and make
execution views feel like native experimental workspaces rather than
embedded utility panels.

## 3. Experiment Workspace

A future Experiment Workspace may unify configuration, execution,
datasets, ML results, reports, and activity under a first-class
experiment entity.

## 4. Provenance Evolution

Future versions may extend the current traceability model toward richer
experimental provenance, linking:

-   research questions;
-   configurations;
-   actions;
-   evidence;
-   transformations;
-   datasets;
-   models;
-   results;
-   conclusions.

## 5. Additional Services and Experimental Environments

CAUSALIS may expand support for additional honeynet and OT/ICS services
while retaining the same controlled-experimentation model.

## 6. APT and Graph-Oriented Experimentation

Future research directions include multi-stage attack scenarios, MITRE
ATT&CK metadata, causal or sequential ground truth, and integration with
graph-oriented analysis workflows such as **ARGOS**.

## 7. Advanced Analytics

Potential extensions include:

-   richer dataset-quality visualizations;
-   per-class multiclass analysis;
-   advanced model-comparison views;
-   interactive experimental timelines;
-   improved provenance visualization;
-   cross-experiment comparison.

The roadmap is research-driven. Items are not commitments to a specific
release date.

------------------------------------------------------------------------

# Branding

Official CAUSALIS assets are stored under:

``` text
app/static/brand/
```

The product identity is:

**Name:** CAUSALIS\
**Descriptor:** Controlled Cyber Experimentation\
**Tagline:** *From controlled action to measurable evidence.*

Repository documentation should use production-ready CAUSALIS brand
assets and repository-relative paths so that the README renders
correctly on GitHub.

------------------------------------------------------------------------

# Documentation

Project documentation may include:

-   `README.md` --- complete project overview;
-   architecture documentation;
-   release notes;
-   validation records;
-   research evidence;
-   dataset documentation;
-   experiment reports.

The README is intended to explain the platform as a whole.
Version-specific implementation details belong in release notes or the
changelog.

------------------------------------------------------------------------

# Citation

CAUSALIS is a research platform developed in the context of doctoral
research on reproducible cybersecurity experimentation, honeynet-based
data generation, dataset engineering, and machine-learning-based attack
detection and attribution.

A definitive bibliographic citation and BibTeX entry should be added
when the associated doctoral thesis and/or peer-reviewed publications
are formally available.

Until then, academic users should reference the repository version used
in their experiment and preserve the corresponding release identifier.

Example:

``` text
CAUSALIS, version 0.9.1, Research Stable.
Controlled Cyber Experimentation.
```

------------------------------------------------------------------------

# Author

**Pedro Díaz García**\
Universidad de Alcalá\
Spain

------------------------------------------------------------------------

# Acknowledgements

CAUSALIS builds on the research and experimental work carried out
around:

-   ThreatSleuth ATG;
-   controlled cyber traffic generation;
-   Generation IV honeynet infrastructures;
-   reproducible cybersecurity dataset generation;
-   experimental ground truth;
-   machine-learning-based attack detection;
-   CAPEC-based attack attribution;
-   reproducible cyber experimentation.

The platform also benefits from the broader research context in which
controlled experimental infrastructures, service telemetry, dataset
engineering, and machine learning are treated as parts of a single
reproducible process.

------------------------------------------------------------------------

# License

The definitive licensing and public-distribution model should be
specified in the repository `LICENSE` file.

Unless a license has already been formally selected for the repository,
no license should be inferred solely from the availability of the source
code.

------------------------------------------------------------------------

```{=html}
<p align="center">
```
`<strong>`{=html}CAUSALIS`</strong>`{=html}`<br>`{=html} Controlled
Cyber Experimentation`<br>`{=html} `<em>`{=html}From controlled action
to measurable evidence.`</em>`{=html}
```{=html}
</p>
```
