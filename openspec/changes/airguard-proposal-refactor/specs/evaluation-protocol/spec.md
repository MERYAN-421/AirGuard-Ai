# Spec Delta

## Purpose

Defines reproducible evaluation evidence for AirGuard forecasting, warnings, decision policies, knowledge grounding, resilience, and user-facing explanations.

## ADDED Requirements

### Requirement: Forecast evaluation uses baselines and defined metrics
Forecast experiments SHALL compare against declared baselines and report MAE and RMSE on chronological holdout data, with results separated by relevant operating conditions.

#### Scenario: Forecast results are reported
- **WHEN** a forecast model is evaluated on held-out data
- **THEN** the report identifies its baseline, data split, MAE, RMSE, and measured results, including applicable condition-specific errors

#### Scenario: Evaluation uses shuffled temporal data
- **WHEN** a proposed test setup randomly mixes future and past observations across train and test
- **THEN** the setup is flagged as invalid for the claimed chronological forecast evaluation

### Requirement: Warning and decision effects are compared under declared conditions
Warning and policy comparisons SHALL define event and warning timing, comparator behavior, common evaluation conditions, and metrics before results are interpreted.

#### Scenario: Warning performance is evaluated
- **WHEN** current-threshold and forecast-based warnings are compared
- **THEN** the report defines event onset and computes lead time, precision, recall, and false-alarm rate from the same declared evaluation period

#### Scenario: Policy effect is evaluated
- **WHEN** an AirGuard policy is compared with a baseline in simulation
- **THEN** both policies use the same simulator and exogenous conditions, and the report labels simulated outcomes and reports exceedance duration, exposure and energy proxies, and switching counts

### Requirement: Evidence status is explicit for all reported targets
The system's evaluation reports SHALL distinguish planned targets, hypotheses, measured results, and unverified claims, and SHALL NOT present targets as achieved results.

#### Scenario: A proposal target is cited
- **WHEN** a target such as warning lead time or reduction in exceedance duration is reported before testing
- **THEN** it is labeled as a target and not as an observed achievement

#### Scenario: A source claim is unverified
- **WHEN** a numerical finding or citation has not been verified against its source
- **THEN** it is excluded from evidence claims or explicitly labeled unverified

### Requirement: Evaluation includes grounding and failure conditions
Evaluation plans SHALL define observable checks for evidence support, unauthorized actions, and degraded operation under data or provider failures.

#### Scenario: Knowledge and safety behavior are tested
- **WHEN** rule and retrieval behavior is evaluated
- **THEN** the protocol records source-support checks, rule applicability, and unauthorized-command outcomes

#### Scenario: A sensor or provider failure is injected
- **WHEN** a test includes stale data, missing values, drift, abnormal readings, or provider outage
- **THEN** the protocol records degradation behavior, false alarms where measurable, and unauthorized physical actions

### Requirement: Knowledge evaluation reports expert agreement status
Knowledge and rule evaluations SHALL report expert agreement when expert review is conducted and SHALL identify the assessment as unavailable when it is not conducted.

#### Scenario: Expert review is conducted
- **WHEN** domain experts assess rule applicability or retrieved support
- **THEN** the evaluation report states the review method and measured agreement

#### Scenario: Expert review is not available
- **WHEN** no domain expert assessment is conducted
- **THEN** the report labels expert agreement as not assessed and makes no expert-consensus claim

### Requirement: Usability evaluation reports declared user measures
When an approved usability study is conducted, the evaluation SHALL report task completion, response time, and the declared rating measure; unavailable studies SHALL NOT be described as completed.

#### Scenario: Approved usability evaluation is conducted
- **WHEN** participants complete the declared comparison tasks
- **THEN** the report includes task completion, response time, rating measure, participant/context information, and study limitations

#### Scenario: Usability study is unavailable
- **WHEN** participants or required approval are unavailable
- **THEN** the report marks usability as not assessed and does not imply user validation
