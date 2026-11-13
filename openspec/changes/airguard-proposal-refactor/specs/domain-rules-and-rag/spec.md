# Spec Delta

## Purpose

Defines auditable, versioned domain-rule evaluation and evidence-linked retrieval so that risk thresholds and explanatory knowledge remain distinct and traceable.

## ADDED Requirements

### Requirement: Risk rules identify scope and authority
Each numeric rule SHALL identify its pollutant or metric, value, unit, averaging period, applicable site or scope, source, and effective version or date.

#### Scenario: Applicable rule is evaluated
- **WHEN** a measurement or forecast is compared against an applicable rule
- **THEN** the result identifies the rule and its scope, unit, averaging period, source, and version

#### Scenario: Rule authority or applicability is unresolved
- **WHEN** the rule source, effective version, or applicability to the site cannot be established
- **THEN** the system marks the result unresolved and does not claim that the site is compliant

### Requirement: Risk classification is determined by auditable rules
The system SHALL derive numeric risk classifications from deterministic rules and their recorded inputs, not solely from generated language-model output.

#### Scenario: Rule condition is met
- **WHEN** valid evidence satisfies a configured rule condition
- **THEN** the system returns the corresponding rule-based risk result with the triggering rule and evidence

#### Scenario: Language-model text conflicts with rule output
- **WHEN** generated explanatory text conflicts with deterministic rule evaluation
- **THEN** the rule result remains authoritative and the conflict is not hidden

### Requirement: Retrieved domain evidence is attributable
The system SHALL return retrievable domain evidence with document identity, version, relevant passage, and source reference, and SHALL disclose when credible supporting material is unavailable.

#### Scenario: Relevant source is retrieved
- **WHEN** a query retrieves a domain document used to explain a recommendation
- **THEN** the returned evidence identifies the document, version, passage, and source reference

#### Scenario: No credible source is found
- **WHEN** retrieval finds no applicable credible evidence
- **THEN** the system states that evidence is unavailable and does not fabricate a citation or claim

### Requirement: Indoor and outdoor standards remain distinct
The system SHALL NOT merge an outdoor ambient-air standard and an indoor-air management guideline into one control threshold without an explicitly verified applicable rule.

#### Scenario: Outdoor standard is retrieved for an indoor decision
- **WHEN** an outdoor standard is the only available threshold for an indoor site
- **THEN** the system identifies the scope mismatch and does not silently apply it as an indoor control limit
