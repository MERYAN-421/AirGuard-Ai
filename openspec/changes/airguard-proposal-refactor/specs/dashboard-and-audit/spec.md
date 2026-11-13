# Spec Delta

## Purpose

Defines user-visible air-quality status and a traceable history that distinguishes observations, forecasts, evidence, decisions, approvals, and execution outcomes.

## ADDED Requirements

### Requirement: Status views distinguish measured and predicted values
The system SHALL present observations and forecasts as distinct data types, with measurement time, forecast issue or target time, units, source, quality status, and update time where applicable.

#### Scenario: Current status is displayed
- **WHEN** a user views current air-quality status
- **THEN** each displayed value identifies whether it is measured or predicted and shows its time, unit, source, and applicable quality information

#### Scenario: Source data is stale
- **WHEN** the latest available input is stale
- **THEN** the view shows its stale state and does not imply a current measurement

### Requirement: Recommendations show their basis and uncertainty
The system SHALL present alert triggers, recommendation rationale, supporting evidence, and material uncertainty or limitations to the user.

#### Scenario: Alert or recommendation is displayed
- **WHEN** the system presents an alert or action recommendation
- **THEN** the user can identify the triggering rule or evidence and any relevant limitation

#### Scenario: No trustworthy evidence is available
- **WHEN** the system cannot establish trustworthy support for an alert explanation
- **THEN** it discloses the evidence gap rather than presenting an unsupported citation

### Requirement: Decision history is auditable
The system SHALL retain an auditable record of the inputs and versions, rules and retrieved evidence, decision, approval, execution mode, and outcome associated with each decision.

#### Scenario: Decision is recorded
- **WHEN** the system issues a risk result, recommendation, or command outcome
- **THEN** the audit record links it to relevant data, model and rule versions, evidence references, approval, and execution status

#### Scenario: Action is rejected or fails
- **WHEN** an action is denied, rejected, simulated, or fails
- **THEN** the audit record preserves that distinct outcome and its reason rather than marking it as successful execution
