# ThreatSleuth Studio

## Baseline Architecture

**Document version:** 0.1\
**Product project:** ThreatSleuth Studio\
**Baseline source:** ThreatSleuth ATG 1.0.0 Stable\
**Baseline package:** `threatsleuth-atg-v1.0.0-final.tar.gz`\
**SHA-256:**
`3dea532986b809511c43993b46d286746d6ae328f029773d6e2216e02b12f50f`\
**Status:** Immutable baseline analysis\
**Date:** 2026-07-09

------------------------------------------------------------------------

## 1. Purpose

This document records the real architecture of the validated
ThreatSleuth ATG 1.0.0 package that serves as the technical baseline for
ThreatSleuth Studio.

The purpose is not to propose a new architecture. It is to document what
currently exists before product-oriented changes begin.

The baseline principle is:

> **DO NOT BREAK WHAT ALREADY WORKS.**

All product development must be evaluated against the validated behavior
documented here.

------------------------------------------------------------------------

## 2. Baseline Package Structure

``` text
threatsleuth-atg/
├── .env
├── .gitignore
├── README.md
├── VERSION
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── static/
│   │   ├── app.js
│   │   └── style.css
│   └── templates/
│       └── index.html
├── data/
│   ├── .gitkeep
│   ├── config/
│   ├── exports/
│   ├── logs/
│   ├── models/
│   └── uploads/
├── deploy.sh
├── requirements.txt
├── run.sh
├── stop.sh
└── test_http_raw.py
```

The application is deliberately compact. Most backend behavior is
implemented in `app/main.py`; the browser client is primarily
implemented in `app/static/app.js`; the current UI is rendered from a
single HTML template.

This compact architecture is functional but creates an important
product-evolution constraint: visual redesign should initially avoid
unnecessary backend decomposition.

------------------------------------------------------------------------

## 3. Runtime Architecture

The baseline is a local web application based on FastAPI and Uvicorn.

``` text
Browser
   │
   │ HTTP / JSON / multipart
   ▼
FastAPI application
app/main.py
   │
   ├── Traffic generation
   ├── Campaign execution
   ├── Scheduler
   ├── Event registration
   ├── Log matching
   ├── Dataset analysis
   ├── Dataset cleaning
   ├── Machine learning
   ├── Intelligent Mode
   └── Reporting / downloads
   │
   ├──────────────► SQLite
   │
   └──────────────► data/ filesystem
```

The frontend uses browser-side JavaScript to call the FastAPI API and
render results dynamically.

No separate frontend framework is used in the baseline.

------------------------------------------------------------------------

## 4. Main Python Data Models

The backend defines Pydantic models for the principal API payloads:

-   `Target`
-   `Campaign`
-   `CampaignAction`
-   `CampaignGroup`
-   `Profile`
-   `MatchReq`
-   `CleanReq`
-   `MLTrainReq`
-   `MLBinaryTrainReq`
-   `MLMultiServiceReq`
-   `IntelligentScanReq`
-   `IntelligentApplyReq`

These models form the validated API contract of the baseline and must be
reviewed before changing request structures.

------------------------------------------------------------------------

## 5. SQLite Persistence

SQLite is the embedded persistence layer.

The connection is created by `db_conn()` with `check_same_thread=False`
and `sqlite3.Row` row mapping.

`init_db()` enables WAL mode:

``` sql
PRAGMA journal_mode=WAL;
```

The validated schema contains seven tables.

### 5.1 `targets`

Stores target systems and services.

Fields:

-   `id`
-   `name`
-   `host`
-   `port`
-   `service`
-   `notes`
-   `created_at`

### 5.2 `profiles`

Stores reusable traffic-generation profiles.

Fields include:

-   `name`
-   `service`
-   `mode`
-   `attack_category`
-   `port`
-   `duration_seconds`
-   `rate_per_second`
-   `concurrency`
-   `path`
-   `username`
-   `password`
-   `dns_query`
-   `payload`
-   `description`
-   `updated_at`

### 5.3 `campaign_groups`

Stores multi-action campaign definitions and execution state.

Fields include:

-   `id`
-   `name`
-   `status`
-   `created_at`
-   `started_at`
-   `finished_at`
-   `scheduled_start`
-   `scheduled_start_epoch`
-   `execute_now`
-   `notes`
-   `actions_total`
-   `actions_finished`
-   `child_campaigns`
-   `progress_pct`
-   `group_json`

The baseline already contains a non-destructive migration that adds
`scheduled_start_epoch` when missing.

### 5.4 `events`

Stores the execution bitácora used as experimental ground truth for
correlation.

Fields include:

-   `id`
-   `timestamp`
-   `timestamp_epoch`
-   `campaign_id`
-   `target_name`
-   `host`
-   `port`
-   `service`
-   `mode`
-   `label`
-   `attack_category_key`
-   `capec_id`
-   `capec_category`
-   `action`
-   `status`
-   `latency_ms`
-   `detail`
-   `profile_name`
-   `matched`
-   `matched_log_line`
-   `raw_json`

This table is regression-critical.

### 5.5 `app_settings`

Stores application configuration as key/value data.

### 5.6 `ml_experiments`

Stores machine-learning experiment metadata.

Fields include:

-   experiment identity and name;
-   dataset filename;
-   target and service columns;
-   dataset dimensions;
-   test size;
-   random seed;
-   creation timestamp;
-   best model;
-   best-service summary;
-   complete configuration JSON.

### 5.7 `ml_results`

Stores per-model experiment results.

Fields include:

-   experiment reference;
-   service;
-   model name;
-   Accuracy;
-   macro Precision;
-   macro Recall;
-   macro F1;
-   weighted F1;
-   MCC;
-   ROC-AUC;
-   training duration;
-   artifact path;
-   confusion matrix JSON;
-   report JSON;
-   creation timestamp.

The existing `ml_experiments` and `ml_results` tables are the primary
data source for the first integrated Results Dashboard.

------------------------------------------------------------------------

## 6. Filesystem Persistence

The baseline uses the `data/` directory for file artifacts.

Principal areas:

-   `data/config/`
-   `data/exports/`
-   `data/logs/`
-   `data/models/`
-   `data/uploads/`

SQLite stores structured application state and experiment metadata. The
filesystem stores uploaded datasets, generated CSV files, model
artifacts, reports, and exports.

ThreatSleuth Studio must initially preserve this hybrid persistence
model.

A future artifact abstraction may be introduced, but moving all
artifacts into SQLite or redesigning storage is not a Phase 1
requirement.

------------------------------------------------------------------------

## 7. Traffic Generation Engine

The traffic engine is centered on four functions:

-   `raw_http(req)`
-   `run_once(req)`
-   `worker(cid, req)`
-   `loop(cid, req)`

### 7.1 `run_once(req)`

`run_once()` is the protocol dispatcher.

It selects service-specific behavior for supported protocols and falls
back to TCP probing where appropriate.

The baseline supports service-oriented generation for HTTP, FTP, SSH,
DNS, SMTP, and additional TCP-oriented services.

### 7.2 `worker(cid, req)`

A worker:

1.  creates a base execution event;
2.  executes one service interaction through `run_once()`;
3.  merges the result into the event;
4.  appends the event to persistent storage.

The generation of network activity and the creation of its execution
record are therefore coupled at the worker boundary.

### 7.3 `loop(cid, req)`

`loop()` controls campaign timing.

It:

-   respects scheduled delay;
-   marks campaign execution as running;
-   calculates request interval from `rate_per_second`;
-   creates concurrent worker threads according to `concurrency`;
-   waits for each concurrent batch;
-   continues until the configured duration is reached or execution is
    stopped.

The validated rate/concurrency semantics must not be silently changed
during UI redesign.

### 7.4 Campaign groups

`group_worker(group_id, group)` executes the actions that compose a
campaign group.

The scheduler and campaign-group model are used to coordinate
multi-action campaigns.

------------------------------------------------------------------------

## 8. Scheduler

Scheduling behavior is implemented through:

-   `_parse_local_datetime()`
-   `_coerce_epoch()`
-   `_schedule_epoch_from_payload()`
-   `_schedule_delay_from_payload()`
-   `_schedule_display_from_epoch()`
-   `db_due_campaign_groups()`
-   `scheduler_loop()`
-   `ensure_scheduler_started()`
-   `scheduler_status()`
-   `scheduler_tick()`

The scheduler was previously sensitive to local time and UTC handling.
The current validated implementation stores and uses
`scheduled_start_epoch`.

Scheduler behavior is regression-critical.

Product changes must preserve:

-   immediate execution;
-   scheduled execution;
-   local-time interpretation;
-   due campaign discovery;
-   visible scheduler state.

------------------------------------------------------------------------

## 9. Log Correlation and Dataset Processing

### 9.1 Log time extraction

`log_time(line)` extracts timestamps from service log lines.

### 9.2 Correlation

`build_match(service, dataset_kind, window_seconds, log_text)` performs
the validated temporal matching process.

API entry points:

-   `/api/match`
-   `/api/match-file`

### 9.3 Temporal window advisor

The baseline contains a temporal window recommendation endpoint:

-   `/api/match/window-advisor`

The source currently contains two definitions of
`temporal_window_advisor()` and two identical route decorators at
different positions.

This duplication should be documented as baseline technical debt. It
must not be removed casually before confirming which definition is
active in FastAPI and validating endpoint behavior.

### 9.4 Dataset analysis

`analyze_rows(rows, fields)` performs quality analysis.

The current frontend renders:

-   missing values;
-   inferred data types;
-   low-variability indicators;
-   domain-logic issues;
-   text-quality issues;
-   strong numeric correlations;
-   class-balance observations.

### 9.5 Dataset cleaning

`clean_dataset(req)` applies the selected cleaning configuration.

The current workflow allows recommended low-variability fields to be
selected for removal and allows the user to override the recommendation.

### 9.6 Split export

`split()` generates train/test dataset exports.

Existing naming and service/Normal/Anomaly conventions are validated
behavior and must be preserved.

------------------------------------------------------------------------

## 10. Machine-Learning Architecture

The ML module is implemented with scikit-learn.

The main training entry points are:

-   `ml_binary_train()`
-   `ml_multi_binary_train()`
-   `ml_capec_multiclass_train()`
-   legacy/general `ml_train()`

Supporting functions include:

-   `_binary_feature_fields()`
-   `_capec_feature_fields()`
-   `_capec_distribution()`
-   `_augment_ml_features()`
-   `_engineered_feature_names()`
-   `_top_model_features()`
-   `_selected_model_names()`
-   `_model_suite()`
-   `_binary_extra_metrics()`
-   `_store_ml_experiment()`

### 10.1 Model suite

`_model_suite(random_seed)` defines the available classifiers.

The validated application compares multiple scikit-learn classifiers
under a common experiment workflow.

### 10.2 Binary experiments

`ml_binary_train()` accepts normal training data, anomaly training data,
normal validation data, and anomaly validation data.

The pipeline constructs binary labels and trains the selected model
suite.

### 10.3 Multi-service binary experiments

`ml_multi_binary_train()` coordinates multiple service-specific binary
jobs.

### 10.4 CAPEC multiclass experiments

`ml_capec_multiclass_train()` builds and trains a multiclass experiment
using CAPEC category as the target.

### 10.5 Metrics

The persisted result model supports:

-   Accuracy;
-   Precision;
-   Recall;
-   F1;
-   weighted F1;
-   MCC;
-   ROC-AUC;
-   training duration;
-   confusion matrix.

### 10.6 Result persistence

`_store_ml_experiment()` persists experiment and result metadata to
SQLite.

This is a key architectural advantage for ThreatSleuth Studio: the first
GUI Results Dashboard can consume existing persisted experiment data
without changing model-training logic.

------------------------------------------------------------------------

## 11. Intelligent Mode

The validated Intelligent Mode is implemented through:

-   `_parse_ports()`
-   `_validate_private_network()`
-   `_tcp_open()`
-   `_profile_templates_for_target()`
-   `_action_from_target_profile()`
-   `intelligent_scan()`
-   `intelligent_apply()`

### 11.1 Discovery

The user supplies a CIDR and port configuration.

The backend validates the network, generates host/port checks, and
performs concurrent TCP connection tests.

### 11.2 Service inference

Discovered services are inferred through known port mappings.

The baseline does not perform banner-based fingerprinting.

### 11.3 Proposal generation

For discovered targets, the application generates service-aware profile
templates and campaign actions.

### 11.4 Apply

The proposed targets, profiles, and campaign are applied through
`intelligent_apply()` after user review.

This review-before-apply behavior must be preserved.

------------------------------------------------------------------------

## 12. Configuration Import and Export

The baseline supports:

-   `/api/config/export`
-   `/api/config/import`

These functions export and import targets and profiles.

Input validation and path-safety behavior are part of the validated
security posture.

ThreatSleuth Studio must not weaken these controls while simplifying the
GUI.

------------------------------------------------------------------------

## 13. Current API Surface

### System

-   `GET /`
-   `GET /api/health`
-   `GET /api/system/status`
-   `GET /api/scheduler/status`
-   `POST /api/scheduler/tick`

### Metadata

-   `GET /api/taxonomy`
-   `GET /api/services`

### Targets and Profiles

-   `GET /api/targets`
-   `POST /api/targets`
-   `DELETE /api/targets/{tid}`
-   `GET /api/profiles`
-   `POST /api/profiles`

### Campaigns

-   `POST /api/campaigns`
-   `POST /api/campaign-groups`
-   `GET /api/campaign-history`
-   `POST /api/test-once`
-   `GET /api/campaigns`
-   `POST /api/campaigns/{cid}/stop`

### Events and Metrics

-   `GET /api/events`
-   `GET /api/metrics`

### Export

-   `GET /api/export`
-   `GET /api/export/split`
-   `GET /api/download/{name}`
-   `GET /api/download/{folder}/{name}`

### Correlation and Data

-   `POST /api/match`
-   `POST /api/match-file`
-   `POST /api/match/window-advisor`
-   `POST /api/dataset/analyze-file`
-   `POST /api/dataset/clean`

### Machine Learning

-   `POST /api/ml/binary-train`
-   `POST /api/ml/multi-binary-train`
-   `POST /api/ml/capec-multiclass-train`
-   `GET /api/ml/experiments`
-   `POST /api/ml/train`
-   `GET /api/ml/experiments/{experiment_id}/report`

### Intelligent Mode

-   `POST /api/intelligent/scan`
-   `POST /api/intelligent/apply`

### Configuration

-   `GET /api/config/export`
-   `POST /api/config/import`

The first product UI redesign should consume this API surface wherever
possible rather than rewriting backend behavior.

------------------------------------------------------------------------

## 14. Frontend Baseline

The current frontend is implemented with:

-   `app/templates/index.html`
-   `app/static/app.js`
-   `app/static/style.css`

`app.js` contains direct DOM manipulation and API calls.

Major frontend functional groups include:

-   input validation;
-   target loading and deletion;
-   profile management;
-   one-off traffic execution;
-   campaign action composition;
-   campaign launch and history;
-   event and metric loading;
-   log matching;
-   temporal-window analysis;
-   dataset analysis rendering;
-   dataset cleaning;
-   Intelligent Mode;
-   ML file handling;
-   binary model training;
-   multi-service batch training;
-   CAPEC training;
-   ML result rendering;
-   ML experiment history;
-   system and scheduler status;
-   campaign progress estimation.

The current frontend already contains significant product logic. A
visual redesign must therefore separate presentation improvements from
behavior changes.

A full frontend-framework migration is not required for Phase 1.

------------------------------------------------------------------------

## 15. Validated End-to-End Workflows

The README and implementation establish the following validated research
workflow:

``` text
Deploy controlled laboratory
        ↓
Configure or discover targets
        ↓
Create traffic profiles
        ↓
Generate normal traffic
        ↓
Generate anomaly traffic
        ↓
Collect service logs
        ↓
Match ATG events with logs
        ↓
Analyze dataset quality
        ↓
Clean datasets
        ↓
Export train/test splits
        ↓
Train binary classifiers
        ↓
Build/train CAPEC multiclass experiment
        ↓
Review and export results
```

Validated experimental services include HTTP, FTP, SSH, and SMTP.

These workflows form the minimum regression scope for ThreatSleuth
Studio.

------------------------------------------------------------------------

## 16. Regression-Critical Behavior

The following behavior must be explicitly validated after
product-oriented changes:

1.  Application deploys successfully using the existing deployment flow.
2.  Application starts and remains usable offline after installation.
3.  Existing SQLite databases are not destroyed.
4.  Targets can be created, loaded, and deleted.
5.  Profiles persist all configured parameters.
6.  Normal profiles do not require CAPEC selection.
7.  Anomaly profiles retain CAPEC metadata.
8.  One-off traffic execution works.
9.  Campaign groups execute all configured actions.
10. Scheduled campaigns execute at the expected local time.
11. Rate and concurrency behavior remains unchanged.
12. Campaign progress remains available.
13. Execution events are persisted.
14. Configuration export/import remains safe and functional.
15. Intelligent Mode scans authorized ranges and generates a reviewable
    proposal.
16. Log files can be uploaded.
17. Temporal correlation works.
18. Window advisor remains functional.
19. Dataset quality analysis works.
20. Recommended and manual feature removal remain available.
21. Clean datasets are generated.
22. Export naming preserves service and Normal/Anomaly context.
23. Train/test split generation works.
24. Binary ML experiments train and persist results.
25. Multi-service binary experiments work.
26. CAPEC multiclass experiments work.
27. Metrics remain numerically consistent.
28. Confusion matrices remain available.
29. ML experiment history remains available.
30. Existing report endpoint remains functional until the new report
    strategy replaces it through a validated migration.

------------------------------------------------------------------------

## 17. Baseline Technical Debt

The following issues are documented without changing them in the
baseline:

### 17.1 Backend concentration

`app/main.py` contains most application responsibilities.

This reduces module isolation but is not itself a reason for immediate
refactoring.

### 17.2 Frontend concentration

`app.js` contains both UI behavior and significant workflow
orchestration.

The product redesign should progressively organize this code, but only
alongside regression validation.

### 17.3 Duplicate temporal-window endpoint definition

Two `temporal_window_advisor()` definitions and duplicate
`/api/match/window-advisor` decorators exist in the baseline source.

Behavior must be tested before cleanup.

### 17.4 Hybrid persistence

SQLite and filesystem artifacts are both required to reconstruct the
complete application state.

Experiment Workspace must initially map over this architecture rather
than assume that all artifacts are database entities.

### 17.5 Report formats

The baseline advertises and generates multiple report/export
representations.

ThreatSleuth Studio will distinguish primary reports from technical
exports and converge primary reporting on HTML and PDF.

------------------------------------------------------------------------

## 18. Phase 1 Architectural Decision

The first implementation phase must not rewrite the traffic engine,
scheduler, correlation engine, cleaning engine, or ML training logic.

Phase 1 should operate primarily on:

-   application shell;
-   navigation;
-   visual design system;
-   Home Dashboard;
-   experiment-oriented presentation;
-   product metadata centralization;
-   integrated read-only ML result visualization.

The safest first analytical feature is the **Results Dashboard**,
because `ml_experiments` and `ml_results` already persist the metrics
and confusion matrices required for visualization.

The first dashboard should therefore read existing persisted results
rather than modify the training pipeline.

------------------------------------------------------------------------

## 19. Immediate Engineering Recommendation

Create the ThreatSleuth Studio repository from the validated baseline
and tag the imported state:

`atg-v1.0.0-baseline`

Then create a product-development branch.

Recommended first implementation sequence:

1.  Centralize product metadata.
2.  Add a new application shell and primary navigation.
3.  Preserve current feature screens as functional views.
4.  Add a read-only Home Dashboard.
5.  Add a read-only ML Results Dashboard backed by existing SQLite
    tables.
6.  Validate all regression-critical workflows.
7.  Only then introduce the Experiment Workspace database model.

This sequence deliberately gives the product an immediate visible
improvement while minimizing risk to the validated backend.

------------------------------------------------------------------------

## 20. Baseline Conclusion

ThreatSleuth ATG 1.0.0 is a compact monolithic web application with a
validated backend that already integrates traffic generation, campaign
orchestration, scheduling, event persistence, log correlation, dataset
engineering, machine-learning experimentation, Intelligent Mode, and
report generation.

The principal opportunity for ThreatSleuth Studio is not to replace this
core. It is to create a product architecture and user experience around
it.

The existing persistence of ML experiments and model results makes
integrated GUI analytics possible with limited backend risk.

Accordingly, the recommended first product milestone is:

> **New product shell + visual redesign + read-only integrated results
> experience, while preserving the validated ATG 1.0.0 execution core
> unchanged.**
