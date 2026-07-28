# CAUSALIS v0.8.1 — Resilient CAPEC Training

## Purpose

This release addresses the loss of long-running CAPEC multiclass training after an application or virtual-machine interruption. It extends the persistent operations architecture introduced in v0.8.0 to CAPEC training and adds deterministic resource cleanup between models.

## Critical changes

- Persistent SQLite-backed CAPEC multiclass tasks.
- Uploaded anomaly datasets are copied to `data/tasks/<task_id>/` before training.
- Progress shows the current model, completed models, total models, elapsed time and ETA.
- Safe pause, resume and controlled cancellation at model boundaries.
- Automatic recovery after application or virtual-machine restart.
- Completed models are detected from task checkpoints and are not trained again.
- Every model result is checkpointed immediately after metrics and model artefacts are persisted.
- The experiment identifier remains stable across recovery.

## Resource lifecycle

After every model, CAUSALIS now performs a best-effort cleanup sequence:

1. persist model artefact;
2. persist metrics and checkpoint state;
3. discard predictions and probability matrices;
4. discard the fitted pipeline and estimator references;
5. close open Matplotlib figures;
6. run Python garbage collection;
7. continue with the next pending model.

This is intended to reduce cumulative RAM pressure and swap activity during multi-hour executions. It does not change the scientific configuration of the classifiers.

## Resource observability

When `psutil` is available, each checkpoint records:

- process resident memory;
- system RAM usage;
- available RAM;
- swap usage;
- process CPU usage;
- peak values observed during the task.

## Compatibility

- Built on the validated CAUSALIS v0.8.0 package.
- Preserves the v0.7.0 research pipeline and existing binary, campaign, processing, dataset, history and report functions.
- The original synchronous CAPEC endpoint remains available for backward compatibility; the GUI uses the persistent task endpoint.

## Recovery semantics

Scikit-learn estimators do not provide a universal mid-fit checkpoint. If interruption occurs while one model is fitting, that single model must restart. All previously completed models remain persisted and are skipped on resume.

## Validation performed

- Python compilation check.
- JavaScript syntax check.
- CAUSALIS v0.7.0 regression suite.
- End-to-end persistent CAPEC smoke test using a synthetic three-class dataset and a Decision Tree model.
