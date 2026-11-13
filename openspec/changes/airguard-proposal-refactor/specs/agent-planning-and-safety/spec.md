# Spec Delta

## Purpose

Defines how the reasoning agent selects evidence-backed recommendations while keeping generated proposals subordinate to deterministic safety checks and human authorization.

## ADDED Requirements

### Requirement: Agent recommendations expose evidence and limitations
The system SHALL associate each recommendation with its relevant observations, forecast, retrieved evidence, rationale, and material uncertainty or data-quality limitation.

#### Scenario: Recommendation has supporting evidence
- **WHEN** the agent proposes an action based on a risk assessment
- **THEN** the recommendation identifies the supporting observations, forecast, retrieved evidence, and rationale

#### Scenario: Supporting evidence is insufficient
- **WHEN** the available evidence is stale, incomplete, or not applicable
- **THEN** the agent reports the limitation and does not present an unsupported recommendation as established fact

### Requirement: Candidate commands pass safety and authorization checks
The system SHALL validate candidate commands against their schema, user permissions, device capabilities, allowed ranges, interlocks, and configured execution mode before any command is eligible for execution.

#### Scenario: Candidate violates a device constraint
- **WHEN** a candidate command is malformed, out of range, mutually incompatible, or unsupported by the device
- **THEN** the system rejects the command and records the reason without dispatching it

#### Scenario: Candidate lacks required permission
- **WHEN** the requesting user or current session lacks the permission required for an action
- **THEN** the system denies execution and records the authorization outcome

### Requirement: Execution defaults to advisory or simulation
The system SHALL default to advisory behavior, SHALL allow simulation without physical dispatch, and SHALL require explicit authorization and any configured human approval before physical actuation.

#### Scenario: Advisory or simulation mode is selected
- **WHEN** the configured execution mode is advisory or simulation
- **THEN** the system provides a recommendation or simulated result and sends no physical device command

#### Scenario: Physical mode lacks approval
- **WHEN** a physical command requires approval and approval has not been granted
- **THEN** the system withholds the command and presents the pending or denied status

#### Scenario: Physical command is approved
- **WHEN** physical mode is explicitly enabled and all permission, device, safety, and approval checks pass
- **THEN** only the validated command may be dispatched and its execution outcome is recorded

### Requirement: Missing evidence and execution faults fail safely
The system SHALL withhold hazardous automatic action when required evidence or device state is unavailable and SHALL record a safe fallback when execution fails or times out.

#### Scenario: Required input is stale or unavailable
- **WHEN** an input required for a safe action is stale, missing, or invalid
- **THEN** the system withholds physical action and requests observation or human review

#### Scenario: Device action fails or times out
- **WHEN** an authorized device action fails or does not complete within its configured timeout
- **THEN** the system records the failure, applies the configured safe fallback if available, and does not report successful execution
