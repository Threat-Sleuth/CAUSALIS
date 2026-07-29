# CAUSALIS v0.9.1 — Research Stable UI Consolidation

This release consolidates the resilient experiment manager introduced in v0.9.0 and completes the general visual and usability pass requested after real binary and CAPEC validation.

## Validated capabilities retained
- Incremental binary and CAPEC results.
- Safe pause, resume and cancellation.
- Persistent checkpoints, recovery and model history.
- Incremental rankings and reports.

## GUI and accessibility improvements
- Reworked experiment monitor with a dark, high-contrast palette.
- Removed light-blue text on grey panels.
- Added semantic status chips, resource metric cards and clearer progress presentation.
- Improved typography, spacing, card hierarchy, tables, controls and focus states.
- Unified CAUSALIS branding and updated visible version label.
- Replaced unavailable resource values with `N/A`.
- Added a useful empty-state message to the execution timeline.
- Improved responsive behaviour for narrow screens.

## Guided Discovery
- Added **Cancel analysis** to **Analyze Environment**.
- Cancellation immediately aborts the client request, restores controls and records a clear cancelled state in the interface.

## Scientific logic
No feature engineering, dataset construction, train/test split, model training or metric-calculation logic was changed.
