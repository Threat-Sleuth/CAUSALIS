# CAUSALIS

![CAUSALIS logo](app/static/brand/causalis_logo_primary.png)

**Controlled Cyber Experimentation**

*From controlled action to measurable evidence.*

---

## Overview

**CAUSALIS** is a platform for controlled and reproducible cybersecurity experimentation.

It integrates traffic generation, campaign orchestration, evidence collection, dataset engineering, machine-learning evaluation, result visualization, and report generation in a single environment.

The project evolved from the validated ThreatSleuth ATG research baseline and is now being developed as an independent product-oriented platform.

---

## Main Capabilities

- Controlled generation of normal and anomalous traffic
- Service-specific experiment design
- Campaign scheduling and execution
- Event registration and ground-truth preservation
- Log ingestion and temporal correlation
- Dataset analysis, cleaning, and construction
- Binary anomaly-detection experiments
- CAPEC-based multiclass classification
- Model comparison using Accuracy, Precision, Recall, F1-score, ROC-AUC, and MCC
- Integrated analytics and confusion matrices
- HTML and PDF report generation
- SQLite persistence
- Offline operation after installation
- Intelligent environment discovery and campaign proposal

---

## Product Vision

CAUSALIS is designed to support the complete cybersecurity experimentation lifecycle:

```text
Environment
    ↓
Targets and Profiles
    ↓
Campaign Execution
    ↓
Traffic and Service Logs
    ↓
Correlation
    ↓
Dataset Engineering
    ↓
Machine-Learning Evaluation
    ↓
Analytics and Reports
```

The long-term product roadmap includes guided experiment workspaces, support for additional services, advanced result visualization, and multi-stage APT scenario orchestration.

---

## Current Status

**Version:** 0.1.2  
**Status:** Early product development  
**Repository:** Private  
**Technical baseline:** ThreatSleuth ATG 1.0.0 Stable

The current version preserves the validated traffic-generation, dataset-processing, and machine-learning core while introducing the CAUSALIS brand, a product-oriented interface, integrated results, and improved reporting.

---

## Core Workflow

### 1. Configure the environment

Define targets manually or discover services through Intelligent Mode.

### 2. Define traffic profiles

Create normal and anomalous traffic profiles adapted to each service.

### 3. Create and run campaigns

Combine targets and profiles into campaigns, execute them immediately, or schedule them.

### 4. Import and correlate logs

Upload service logs and correlate them with CAUSALIS execution events.

### 5. Engineer datasets

Analyze quality, select features, clean records, and generate training and testing datasets.

### 6. Train and evaluate models

Run binary or multiclass classification experiments and compare several machine-learning algorithms.

### 7. Analyze results

Inspect metrics, rankings, confusion matrices, class distributions, and experiment history directly in the GUI.

### 8. Generate reports

Create professional HTML, PDF, or combined report packages.

---

## Main Modules

### Home

Operational dashboard with system status, scheduler state, recent activity, experiment readiness, event distribution, and ML summaries.

### Experiment Designer

Management of targets, traffic profiles, and campaigns.

### Intelligent Mode

Network discovery, service identification, profile proposal, and automatic campaign generation.

### Data Engineering

Log correlation, dataset analysis, quality assessment, cleaning, and split generation.

### Machine Learning

Binary, multi-service, and CAPEC multiclass experiments.

### Analytics and Reports

Integrated metrics, model rankings, confusion matrices, visual summaries, and report generation.

---

## Architecture

```text
Browser
   │
   ▼
FastAPI Web Application
   │
   ├── Experiment and Campaign Management
   ├── Traffic Generation Engine
   ├── Scheduler
   ├── Intelligent Mode
   ├── Event Registration
   ├── Log Correlation
   ├── Dataset Engineering
   ├── Machine-Learning Evaluation
   ├── Analytics
   └── Reporting
   │
   ├── SQLite
   └── File-Based Artifacts
```

Detailed architecture documentation is available in:

```text
docs/arquitectura.md
```

---

## Technology Stack

- Python
- FastAPI
- Uvicorn
- SQLite
- scikit-learn
- ReportLab
- Vanilla JavaScript
- HTML and CSS
- Bash deployment scripts

---

## Repository Structure

```text
CAUSALIS/
├── app/
│   ├── main.py
│   ├── static/
│   │   ├── app.js
│   │   ├── style.css
│   │   └── brand/
│   └── templates/
│       └── index.html
├── data/
│   ├── config/
│   ├── exports/
│   ├── logs/
│   ├── models/
│   └── uploads/
├── docs/
│   └── arquitectura.md
├── deploy.sh
├── run.sh
├── stop.sh
├── requirements.txt
├── VERSION
├── CHANGELOG.md
└── README.md
```

---

## Installation

CAUSALIS is currently tested on Kali Linux in a controlled laboratory environment.

### 1. Extract the package

```bash
tar -xzf causalis-v0.1.2-initial.tar.gz
cd CAUSALIS
```

### 2. Deploy

```bash
chmod +x deploy.sh run.sh stop.sh
./deploy.sh
```

### 3. Run

```bash
./run.sh
```

### 4. Open the application

Use the URL shown by the startup script.

---

## Existing Database Compatibility

CAUSALIS preserves compatibility with the validated SQLite database used by the original research baseline.

The database path remains:

```text
data/atg.db
```

Before restoring a previous database:

```bash
./stop.sh
cp /path/to/backup/atg.db data/atg.db
./run.sh
```

Always stop the application before copying the SQLite database.

---

## Responsible Use

CAUSALIS is intended exclusively for:

- Authorized cybersecurity research
- Teaching
- Dataset generation
- Security laboratories
- Controlled testing environments

Do not use CAUSALIS against systems or networks without explicit authorization.

---

## Reporting

Primary report formats:

- HTML
- PDF
- HTML and PDF combined

Technical data exports such as CSV or JSON, when available, are treated separately from user-facing reports.

---

## Roadmap

### Product Experience

- Complete GUI redesign
- Experiment Workspace
- Guided Mode and Advanced Mode
- Improved Home dashboard
- Redesigned Experiment Designer
- Simplified Data Engineering workflow
- Improved Machine Learning workflow
- Enhanced Intelligent Mode

### Analytics

- Advanced model-comparison charts
- Interactive confusion matrices
- Dataset quality visualizations
- Training-time versus performance analysis
- Per-class multiclass metrics
- Unified activity timeline

### Platform Evolution

- Support for additional honeynet services
- Experiment import and export
- Backup and restore
- Enhanced PDF reports
- APT Scenario Engine
- MITRE ATT&CK integration
- Ground-truth generation for graph-based APT research
- Distributed execution agents
- Multi-user capabilities

---

## Scientific Background

CAUSALIS is derived from a research framework created to generate reproducible cybersecurity datasets and evaluate hierarchical two-phase cyberattack detection and attribution.

Associated research lines include:

- Generation IV Honeynet infrastructures
- Controlled traffic generation
- Cybersecurity dataset engineering
- Binary anomaly detection
- CAPEC-based attack attribution
- Reproducible machine-learning experiments
- Graph-based representation and detection of APT campaigns

---

## Citation

A formal citation and BibTeX entry will be added when the associated publications are finalized.

---

## Documentation

Current project documentation includes:

- Product specification
- Baseline architecture
- GUI and product specification
- Architecture document
- Release notes
- Validation records

Recommended repository layout:

```text
docs/
├── product/
├── architecture/
└── validation/
```

---

## Branding

Official CAUSALIS brand assets are stored in:

```text
app/static/brand/
```

The repository should contain:

- Primary logo for light backgrounds
- Primary logo for dark backgrounds
- Reduced logo variants
- Corporate isotype for light backgrounds
- Corporate isotype for dark backgrounds
- Monochrome variants

---

## Development Principles

CAUSALIS follows two fundamental rules:

1. **Do not break what already works.**
2. **Every interface element must have a clear purpose.**

Validated backend functionality is preserved while the user experience evolves incrementally.

---

## Author

**Pedro Díaz García**  
Universidad de Alcalá

---

## Acknowledgements

CAUSALIS builds on the research and experimental work conducted around ThreatSleuth ATG, Generation IV Honeynet infrastructures, cybersecurity dataset generation, and machine-learning-based attack detection.

---

## License

The licensing model has not yet been finalized.

CAUSALIS is currently under private development.
