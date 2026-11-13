# Spec Delta

## Purpose

Defines observable quality labeling and safe fallback behavior when observations are missing, implausible, delayed, unsynchronized, or otherwise insufficient for current analysis.

## ADDED Requirements

### Requirement: Data quality issues are explicitly identified
The system SHALL identify missing, out-of-range, abrupt-change, stale, and time-alignment quality concerns using recorded quality flags or equivalent explicit annotations.

#### Scenario: Measurement is missing or stale
- **WHEN** an expected measurement is absent or older than the configured freshness limit
- **THEN** the affected data is marked missing or stale and is excluded from claims of current valid measurement

#### Scenario: Measurement is implausible or abruptly changes
- **WHEN** a value violates configured plausibility bounds or abrupt-change checks
- **THEN** the system marks it suspect or invalid and preserves the observation for traceability rather than silently treating it as normal

### Requirement: Quality insufficiency limits downstream claims
The system SHALL disclose when the available observations are insufficient for a forecast or risk assessment and SHALL NOT use stale or invalid input as an unqualified current guarantee.

#### Scenario: Analysis input is insufficient
- **WHEN** required inputs are missing, invalid, stale, or inadequately aligned
- **THEN** the system reports the limitation and produces an unknown or otherwise conservative outcome instead of an unqualified forecast or risk claim

#### Scenario: Outdoor proxy is not a substitute for indoor truth
- **WHEN** indoor CO2 or TVOC observations are unavailable
- **THEN** the system does not present outdoor PM2.5 as an indoor CO2 or TVOC measurement

### Requirement: Suspected sensor faults are distinguishable from pollution events
The system SHALL preserve the distinction between evidence of a possible sensor fault and evidence of a possible pollution event.

#### Scenario: Sensor anomaly is detected
- **WHEN** an anomalous value has not been established as a true environmental event
- **THEN** the system records the uncertainty as a suspected sensor fault or pollution event without treating the labels as interchangeable

#### Scenario: Calibration is not validated
- **WHEN** a correction method lacks reference-measurement validation for the relevant conditions
- **THEN** the system does not represent corrected data as verified ground truth
