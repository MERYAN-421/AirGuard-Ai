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

### Requirement: Pollution events use predeclared operational definitions
The evaluation SHALL declare the site, pollutant, threshold, averaging period, valid sampling grid, and persistence condition before scoring pollution events.

#### Scenario: Event onset is scored
- **WHEN** two consecutive valid averaging windows exceed the predeclared threshold
- **THEN** event onset is the timestamp of the first of those windows

#### Scenario: Event window contains invalid data
- **WHEN** a required averaging window is missing or invalid
- **THEN** the affected event interval is marked unevaluable and missing values are not interpolated to create an event

### Requirement: Warning scores use a fixed 30-minute forecast horizon
The evaluation SHALL issue a forecast warning only when the forecast predicts threshold exceedance at issue time plus 30 minutes and compute event lead time within the fixed pre-onset interval.

#### Scenario: Forecast warning precedes an event
- **WHEN** the forecast at issue time predicts threshold exceedance at issue time plus 30 minutes and is issued from 30 minutes before event onset up to, but not including, onset
- **THEN** lead time is event onset minus the earliest qualifying warning time and is reported as no greater than 30 minutes

#### Scenario: Event has no qualifying warning
- **WHEN** no qualifying warning is issued in the 30 minutes before an event
- **THEN** the event is counted as a false negative

#### Scenario: Warning is not followed by an event
- **WHEN** a warning episode is not followed by an event onset for the same site and pollutant within 30 minutes
- **THEN** the warning episode is counted as a false alarm

### Requirement: Exceedance reduction uses paired simulations
The evaluation SHALL compare durations, defined as summed valid simulation intervals above the threshold, under matched initial state, external conditions, action constraints, and random seed.

#### Scenario: Baseline and AirGuard durations are compared
- **WHEN** both policies are simulated under the same declared scenario
- **THEN** the report provides absolute durations and calculates reduction as 100 × (baseline duration − AirGuard duration) / baseline duration

#### Scenario: Baseline has no exceedance duration
- **WHEN** the baseline exceedance duration is zero
- **THEN** percentage reduction is reported as undefined or not computable, not as zero or 100 percent

### Requirement: RAG and IAQ-rule ablations retain hard safety controls
The evaluation SHALL compare the full system, no-RAG, no-IAQ-decision-rules, and neither variants while keeping physical-dispatch prohibition and hard safety checks enabled in every arm.

#### Scenario: Ablation variant is evaluated
- **WHEN** a RAG or IAQ decision-rule component is omitted for comparison
- **THEN** the report identifies the omitted component and retains permissions, device limits, interlocks, and physical-dispatch prohibition

#### Scenario: Ablation proposes an unsafe command
- **WHEN** an ablation variant proposes a command that fails a hard safety check
- **THEN** the command is blocked and recorded rather than dispatched

### Requirement: Simulation assumptions and parameter provenance are reported
The evaluation SHALL identify its simulator structure, parameter sources or declared assumptions, common exogenous inputs, and material limitations; simulated outcomes SHALL NOT be reported as field results.

#### Scenario: Simulation results are reported
- **WHEN** a policy result is computed in the proposed single-zone simulator
- **THEN** the report labels it simulated and documents parameter provenance, shared scenario inputs, and limitations relevant to interpretation

#### Scenario: A simulation parameter is unavailable
- **WHEN** a required parameter has no site measurement or verified source
- **THEN** the report labels the assumption and analyzes sensitivity rather than presenting it as measured
