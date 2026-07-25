# CAUSALIS v0.7.0 — Reliable Dataset Splits and Simplified ML Operations

## Purpose

This release addresses issues discovered during the final full-service doctoral-thesis experiment while preserving the validated v0.6.0 research pipeline.

## Fixed

- Correct parsing of `clean_<service>_<kind>_<timestamp>.csv` when the service contains underscores.
- Correct REST API split generation: normal/anomaly × training/test.
- Correct `wireguard_ui` split naming for future use.
- Offset-less ISO HTTP timestamps are interpreted as UTC during correlation.
- PostgreSQL filename aliases now include `database`.

## Added

- Single-folder discovery for binary training datasets.
- Pre-training completeness table for every service.
- Sequential automatic binary training across all ready services.
- Single-folder anomaly discovery for CAPEC attribution.
- Clear reporting of incomplete service sets.

## Compatibility

- Manual binary training remains unchanged.
- Manual CAPEC selection remains unchanged.
- Existing API endpoints are preserved.
- Existing model suites, features, metrics, persistence and reports are preserved.

## Validation performed

- Python syntax compilation.
- JavaScript syntax validation.
- Regression test confirming four correctly named REST API split files.
- Regression test confirming UTC epoch conversion for offset-less HTTP timestamps.

## Known limitation

Useful VPN/WireGuard telemetry was not obtained during the thesis campaign and VPN was excluded from the final experimental dataset.
