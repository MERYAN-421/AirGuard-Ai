# Spec Delta

## Purpose

Defines reproducible short-horizon indoor air-quality forecasts whose target, issue time, model provenance, input quality, and uncertainty limitations are explicit.

## ADDED Requirements

### Requirement: Forecasts identify target and model provenance
The system SHALL identify each forecast's site, target pollutant, issue time, target time or horizon, predicted value and unit, model name and version, and input data quality.

#### Scenario: Forecast is produced
- **WHEN** a forecast is generated from an eligible observation window
- **THEN** its output identifies the forecast target and horizon, issue time, model version, value, unit, and input quality

#### Scenario: Required indoor target observations are unavailable
- **WHEN** a requested target depends on an indoor measurement stream that is not available
- **THEN** the system reports that the forecast cannot be supported by available input and does not substitute an unrelated outdoor metric as indoor truth

### Requirement: Supported indoor forecasts use the proposed horizon
The system SHALL support a 30-minute forecast for indoor PM2.5 when suitable input is available; a CO2 forecast requires indoor CO2 observations.

#### Scenario: Indoor PM2.5 forecast is requested
- **WHEN** a site has a valid indoor PM2.5 observation window and a forecast is requested
- **THEN** the forecast target time is 30 minutes after its issue time

#### Scenario: CO2 forecast lacks indoor observations
- **WHEN** a CO2 forecast is requested but no valid indoor CO2 observation window is available
- **THEN** the system reports the target as unsupported and does not substitute outdoor data

### Requirement: Forecast uncertainty is described without unsupported guarantees
The system SHALL distinguish predicted values from measured values and SHALL label uncertainty ranges according to their actual validation status.

#### Scenario: Forecast is displayed
- **WHEN** a forecast is presented alongside observations
- **THEN** the forecast is visibly distinguishable from measured values and identifies its target time

#### Scenario: Uncalibrated uncertainty range is available
- **WHEN** an uncertainty range is derived without coverage calibration
- **THEN** the system labels it as an empirical range and does not claim guaranteed confidence-interval coverage

### Requirement: Forecast evaluations use time-respecting inputs
The system SHALL support evaluation records that identify the data window, chronological split, model parameters, random seed when applicable, code version, and metric definitions.

#### Scenario: Forecast experiment is recorded
- **WHEN** a forecast model is evaluated
- **THEN** the experiment record contains enough configuration and version information to reproduce its inputs and metric computation

#### Scenario: Future information could leak into training
- **WHEN** a proposed split or transformation would expose future test observations to training or fitted preprocessing
- **THEN** that evaluation setup is rejected or flagged as invalid rather than reported as a valid test result
