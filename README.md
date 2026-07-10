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
Experimentation`</strong>`{=html}`<br>`{=html} From controlled action to
measurable evidence.
```{=html}
</p>
```
CAUSALIS is a product-oriented cybersecurity experimentation and dataset
engineering platform. It preserves the validated ThreatSleuth ATG 1.0.0
research core and adds a new branded product experience, integrated
experiment analytics, visual result exploration, and unified HTML/PDF
report generation.

This is the **initial CAUSALIS package, version 0.1.2**.

> **Core rule:** DO NOT BREAK WHAT ALREADY WORKS.

The validated traffic-generation, campaign, scheduling, correlation,
cleaning, dataset-construction and machine-learning workflows remain
compatible with the previous stable core.

------------------------------------------------------------------------

## 1. Product Scope

CAUSALIS supports a complete controlled experimentation workflow:

``` text
Configure or discover targets
        ↓
Generate normal and anomalous traffic
        ↓
Collect service logs
        ↓
Correlate execution evidence and logs
        ↓
Assess and clean datasets
        ↓
Create train/test datasets
        ↓
Train binary and CAPEC multiclass models
        ↓
Explore results in the GUI
        ↓
Generate HTML and/or PDF reports
```

The product is intended for:

-   cybersecurity research;
-   security laboratories and R&D teams;
-   university teaching and training;
-   dataset engineering;
-   authorized validation of detection models;
-   future multi-stage APT experimentation.

CAUSALIS is **not** intended for unauthorized scanning, exploitation, or
use against public or third-party systems.

------------------------------------------------------------------------

## 2. Main Capabilities

### Traffic and campaign orchestration

-   target and profile management;
-   normal and anomaly traffic generation;
-   one-off actions;
-   multi-action campaigns;
-   scheduled execution;
-   rate and concurrency control;
-   persistent execution evidence;
-   authorized Intelligent Mode discovery.

### Dataset engineering

-   service-log upload;
-   temporal event correlation;
-   temporal-window advisor;
-   missing-value and duplicate analysis;
-   entropy and variability indicators;
-   outlier and domain-quality checks;
-   configurable cleaning;
-   train/test split export;
-   binary and CAPEC dataset construction.

### Machine learning

-   service-specific binary classification;
-   multi-service sequential batches;
-   CAPEC multiclass classification;
-   multiple scikit-learn classifiers;
-   Accuracy, Precision, Recall, F1, MCC, ROC-AUC and balanced metrics;
-   confusion matrices;
-   persisted experiment history in SQLite.

### Integrated analytics

-   Home dashboard;
-   recent traffic, ML and report activity;
-   experiment readiness indicators;
-   event distribution;
-   model ranking by MCC;
-   best-model metric profile;
-   confusion-matrix heatmap;
-   class distribution;
-   training-time versus performance visualization;
-   detailed metric tables.

### Reporting

The primary report formats are:

-   **HTML** --- portable and browser-ready;
-   **PDF** --- fixed layout for sharing and archival;
-   **HTML + PDF** --- generated together as a ZIP file.

JSON and CSV are technical data exports, not primary reports.

------------------------------------------------------------------------

## 3. Installation

### Requirements

-   Kali Linux or another Debian-compatible Linux distribution;
-   Python 3;
-   Internet access during the first deployment to install dependencies;
-   network access to an explicitly authorized laboratory environment.

### Deploy

``` bash
chmod +x deploy.sh run.sh stop.sh
./deploy.sh
```

The deployment script:

1.  installs required system packages;
2.  creates a local `.venv`;
3.  installs Python dependencies;
4.  creates persistent data directories;
5.  preserves the application as an offline-capable local deployment.

### Start

``` bash
./run.sh
```

Open:

``` text
http://<HOST_IP>:8080
```

The port can be changed with:

``` bash
CAUSALIS_PORT=8081 ./run.sh
```

For backward compatibility, the legacy `ATG_PORT` environment variable
is also accepted.

### Stop

``` bash
./stop.sh
```

------------------------------------------------------------------------

## 4. User Workflow

### 4.1 Home

Home presents:

-   configured targets and profiles;
-   event and ML experiment counts;
-   experiment readiness;
-   recent traffic, model-training and report activity;
-   best model snapshots;
-   service and mode distributions;
-   a non-blocking class-balance observation when one mode dominates
    recorded events.

### 4.2 Traffic

Create targets and profiles manually or use Intelligent Mode in an
authorized network. Campaigns can contain multiple actions and may
execute immediately or at a scheduled time.

### 4.3 Data

Upload service logs and correlate them with CAUSALIS execution events.
Review quality analysis, apply cleaning decisions and create reusable
datasets.

### 4.4 Models

Train:

-   binary Normal/Attack models;
-   multi-service binary batches;
-   CAPEC multiclass models.

The GUI warns when filenames appear to belong to a different service
than the configured batch service. The warning does not block expert
workflows.

### 4.5 Results

Select an experiment to review:

-   best model;
-   core metrics;
-   model ranking;
-   confusion matrix;
-   class distribution;
-   training-time/performance relationship;
-   detailed results.

Use **Generate report** to select HTML, PDF, or both.

------------------------------------------------------------------------

## 5. Data and Persistence

CAUSALIS uses SQLite and filesystem artifacts.

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

The database filename remains `atg.db` for compatibility with validated
installations. It stores targets, profiles, campaign groups, execution
events, application settings, ML experiments and ML results.

Before replacing or upgrading an installation, stop the application and
back up:

``` bash
cp data/atg.db ~/causalis_atg.db.backup
```

Restore it only while CAUSALIS is stopped.

------------------------------------------------------------------------

## 6. Branding Assets

Production assets are located in:

``` text
app/static/brand/
```

The package includes:

-   primary logo;
-   horizontal logo;
-   compact logo;
-   monochrome variants;
-   isotype;
-   favicon and application icons.

Product name, version, descriptor and tagline are centralized in
`app/main.py` and shared by the GUI and generated reports.

------------------------------------------------------------------------

## 7. Responsible Use

CAUSALIS is intended exclusively for research, teaching, dataset
generation and authorized laboratory testing.

Do not use it against systems or networks without explicit
authorization.

Intelligent Mode performs TCP connection checks and must be restricted
to private or expressly authorized ranges. The operator remains
responsible for legal authorization, network isolation and safe
experimental design.

------------------------------------------------------------------------

## 8. Version 0.1.2 Highlights

-   complete CAUSALIS branding throughout GUI and reports;
-   branded application header, favicon, footer and dialogs;
-   integrated Results analytics;
-   HTML/PDF/both report selector;
-   branded self-contained HTML reports;
-   branded PDF reports;
-   recent report activity on Home;
-   class-balance observation on Home;
-   service/filename mismatch warnings in multi-service ML batches;
-   preserved validated ATG 1.0.0 core behavior;
-   rewritten product README and architecture specification.

See [`architecture.md`](architecture.md) for the technical architecture
and compatibility constraints.

------------------------------------------------------------------------

## 9. Current Roadmap

1.  validate CAUSALIS 0.1.2 end to end;
2.  refine the visual system and result interactions;
3.  introduce Experiment Workspace;
4.  simplify the guided data workflow;
5.  extend support from four to eight honeynet services;
6.  introduce APT Scenario Engine, ATT&CK metadata and ground-truth
    sequences;
7.  prepare product licensing, packaging and commercial positioning.

------------------------------------------------------------------------

## 10. License and Distribution

This repository is currently private. Licensing, commercial distribution
and public-release terms have not yet been finalized.
