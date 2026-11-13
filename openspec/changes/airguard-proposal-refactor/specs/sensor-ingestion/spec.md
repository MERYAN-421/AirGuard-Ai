# Spec Delta

## Purpose

Defines how AirGuard accepts, identifies, timestamps, and normalizes indoor and outdoor air-quality observations while preserving their source values and provenance.

## ADDED Requirements

### Requirement: Indoor observations have traceable measurement fields
The system SHALL accept an indoor observation only when it can associate the observation with a sensor, site, observation time, metric, value, unit, and quality status.

#### Scenario: Complete indoor observation
- **WHEN** an indoor measurement contains a sensor ID, site ID, timestamp, metric, numeric value, unit, and quality status
- **THEN** the system records the observation with those fields available for downstream use

#### Scenario: Required field is absent
- **WHEN** an indoor measurement omits a required identity, time, or measurement field
- **THEN** the system rejects or isolates that measurement and does not present it as a valid observation

### Requirement: Outdoor observations retain provider provenance and freshness
The system SHALL retain the external provider, station or location, measurement time, and retrieval time for each outdoor observation, and SHALL mark unavailable or stale provider data as stale.

#### Scenario: Outdoor data is retrieved
- **WHEN** an external provider returns an observation
- **THEN** the recorded observation identifies its provider and location and distinguishes measurement time from retrieval time

#### Scenario: Outdoor provider is unavailable
- **WHEN** a provider request fails or its returned data is stale under the configured freshness policy
- **THEN** the affected source is marked stale rather than represented as a current observation

### Requirement: Normalization preserves original measurements
The system SHALL normalize accepted timestamps and units for comparison while retaining the original value and enough conversion metadata to trace the normalized value to its source.

#### Scenario: Observation is converted
- **WHEN** an accepted observation uses a supported noncanonical unit or timezone
- **THEN** the system records a normalized value and timestamp without silently replacing the original measurement

#### Scenario: Conversion is not defined
- **WHEN** the system cannot safely interpret an observation's unit or timestamp
- **THEN** it flags or isolates the observation and does not silently infer a converted value
