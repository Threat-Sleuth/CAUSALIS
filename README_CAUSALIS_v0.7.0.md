# CAUSALIS v0.7.0

**Controlled Cyber Experimentation**  
*From controlled action to measurable evidence.*

CAUSALIS is a reproducible research platform for generating controlled normal and anomalous traffic, correlating service evidence with experimental ground truth, engineering labelled datasets, training machine-learning models, and producing traceable research reports.

Version 0.7.0 preserves the validated CAUSALIS v0.6.0 experimentation core and adds targeted reliability and usability improvements discovered during the final doctoral-thesis campaign.

## What is new in v0.7.0

- Fixed dataset-split export for service names containing underscores, especially `rest_api` and `wireguard_ui`.
- REST API now exports distinct normal/anomaly training and test files instead of misclassifying `api` as the dataset kind.
- Offset-less ISO-8601 timestamps in HTTP logs are interpreted as UTC during correlation. Source logs are not modified.
- PostgreSQL evidence detection accepts `postgresql`, `postgres`, `database`, and `db` aliases.
- Binary model training can now start from a single `dataset_splits` folder.
- CAUSALIS automatically discovers and validates all four files required per service:
  - normal training;
  - anomaly training;
  - normal test;
  - anomaly test.
- CAPEC attribution can use the same folder and automatically select anomaly split files.
- Existing manual four-file binary training and manual CAPEC file selection remain available for backward compatibility.
- Incomplete service datasets are reported before training and skipped only after user confirmation.

## Validated scientific workflow

```text
Traffic campaigns
    ↓
Ground-truth event log
    ↓
Service evidence import
    ↓
Temporal correlation
    ↓
Clean labelled datasets
    ↓
Training/test split package
    ↓
Binary detection: normal vs attack
    ↓
CAPEC attribution: attack category
    ↓
Metrics, models and reports
```

## Installation

### Requirements

- Linux host or virtual machine
- Python 3.10 or later
- Internet access during first dependency installation
- Recommended: 8 GB RAM or more for full model profiles

### Deploy

```bash
chmod +x deploy.sh run.sh stop.sh
./deploy.sh
```


### Start

```bash
./run.sh
```

Open the URL printed by the launcher, normally:

```text
http://127.0.0.1:8080
```

### Stop

```bash
./stop.sh
```

## Core workflow

### 1. Configure targets and profiles

Define the normal and anomaly endpoints of the research infrastructure. CAUSALIS includes service-aware traffic generation for HTTP, FTP, SSH, mail, PostgreSQL, REST API, SMB and other supported families.

### 2. Run controlled campaigns

Campaigns record experimental ground truth in `data/logs/events.jsonl`. Each event includes service, mode, CAPEC category, timestamps, status and campaign metadata.

### 3. Import and process evidence

Import service logs, select the correlation window, and generate matched evidence. The original evidence remains unchanged.

For HTTP logs with timestamps such as:

```text
2026-07-24T06:04:20.145833
```

CAUSALIS v0.7.0 treats the value as UTC internally:

```text
2026-07-24T06:04:20.145833+00:00
```

Explicit offsets remain authoritative.

### 4. Generate clean datasets

Use the Data Engineering workspace to analyse quality, remove exact duplicates when required, and export clean datasets.

### 5. Generate the Training/Test Package

The split exporter creates a ZIP with this structure:

```text
dataset_splits/
├── ftp/
│   ├── ftp_normal_training.csv
│   ├── ftp_normal_test.csv
│   ├── ftp_anomaly_training.csv
│   └── ftp_anomaly_test.csv
├── rest_api/
│   ├── rest_api_normal_training.csv
│   ├── rest_api_normal_test.csv
│   ├── rest_api_anomaly_training.csv
│   └── rest_api_anomaly_test.csv
└── ...
```

The split is deterministic by current row order and uses approximately 80% for training and 20% for testing.

## Simplified machine-learning workflow

### Binary detection

1. Open **Machine Learning → Binary Detection**.
2. Select the extracted `dataset_splits` folder.
3. Click **Validate Folder**.
4. Review the service table.
5. Select the model profile.
6. Click **Train All Binary Services**.

CAUSALIS runs services sequentially to reduce memory pressure. Each service must contain both classes in training and test data.

### CAPEC attribution

1. Open **Machine Learning → CAPEC Attribution**.
2. Select the same `dataset_splits` folder.
3. CAUSALIS selects all anomaly training/test files automatically.
4. Choose the CAPEC target and model profile.
5. Click **Train CAPEC Models from Folder**.

Only anomaly records with a valid CAPEC target are used. Target and alternative CAPEC label fields are excluded from predictors to reduce target leakage.

### Manual compatibility mode

The original workflows remain available:

- four-file selection for one binary service;
- manual selection of anomaly CSV files for CAPEC attribution;
- manual batch queue.

This preserves compatibility with v0.6.0 experiments and custom external datasets.

## Model profiles

- **FAST**: recommended for rapid validation and large multi-service batches.
- **BALANCED**: broader comparison with moderate execution cost.
- **FULL RESEARCH**: all available classifiers, including heavier models.
- **CUSTOM**: manual model selection.

Available classifiers include Decision Tree, Random Forest, Extra Trees, Logistic Regression, Naive Bayes, KNN, Gradient Boosting, AdaBoost, Histogram Gradient Boosting, SVM and MLP.

## Output directories

```text
data/
├── logs/
├── config/
├── uploads/
├── models/
├── experiments/
└── exports/
    ├── matched_logs/
    ├── clean_datasets/
    ├── dataset_splits/
    ├── ml_datasets/
    └── ml_reports/
```

## Reproducibility recommendations

- Preserve the original evidence ZIP and `events.jsonl`.
- Keep clean datasets and split datasets as immutable experiment artefacts.
- Record the CAUSALIS version, honeynet version, random seed, correlation window and model profile.
- Do not overwrite final thesis outputs. Use a dated experiment directory.
- Treat new runs with later software versions as separate experimental iterations.

## Known limitation

The doctoral-thesis campaign did not obtain useful VPN/WireGuard telemetry, so VPN was excluded from the final dataset. The service remains available for future development, but it should not be treated as validated evidence generation in this release.

## Backward compatibility

CAUSALIS v0.7.0 keeps:

- existing API endpoints;
- existing manual ML workflows;
- SQLite experiment history;
- validated feature extraction and model suites;
- existing reports and artefact directories;
- campaign, processing and correlation behaviour, except for the explicit UTC interpretation of offset-less HTTP timestamps.

## Version

```text
0.7.0
```

See `RELEASE_NOTES_CAUSALIS_v0.7.0.md` and `CHANGELOG.md` for release details.
