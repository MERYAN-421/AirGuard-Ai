# AirGuard AI

**A proactive indoor air-quality protection agent integrating IoT sensor streams, forecasting, domain knowledge, and safety-constrained decision support.**

> **Project status: Proposed / not yet implemented.** The architecture, technology choices, evaluation protocol, and quantitative targets in this repository are plans. No AirGuard application has been implemented, and no AirGuard experiments or measured results are available.

## Overview

AirGuard AI is a research proposal for connecting indoor air-quality observations to explainable, evidence-grounded recommendations. The proposed workflow combines sensor-data quality checks, short-horizon forecasting, versioned domain rules, retrieval of relevant guidance and equipment information, and simulated action evaluation.

The project is intended to study whether this combination can provide useful warnings without overstating sensor evidence, confusing indoor and ambient-air guidance, or granting a language model authority over safety-critical decisions.

## Proposed Architecture

The design retains eight modules:

1. **Context & UI** — site configuration, user requests, permissions, and presentation.
2. **Input Ingestion** — message validation and normalization of timestamps and units.
3. **Planner & Reasoning Agent** — task decomposition and selection of available tools.
4. **Data Acquisition & Perception** — indoor sensor, selected outdoor-provider, and historical observations.
5. **Analytics Engine** — data-quality checks, validated calibration where available, forecasting, and anomaly evidence.
6. **Domain Knowledge & Rules** — versioned deterministic rules and attributed retrieval of relevant documents.
7. **Decision & Actuation** — candidate actions, device constraints, safety checks, approval, and simulation.
8. **Output & Audit** — separate presentation of measurements and forecasts, with traceable evidence and outcomes.

See [DESIGN.md](DESIGN.md) for the architecture diagram, workflow, data contracts, and design rationale.

## Scope and Safety

The proposed MVP covers monitoring, forecasting, explanations, and simulated device commands. **Physical HVAC/device control is outside the MVP scope.** Advisory behavior is the default; hard safety checks and authorization are not delegated to an LLM. Numeric risk classification is intended to come from auditable deterministic rules.

Indoor PM2.5 is the proposed first forecast target. CO2 forecasting requires actual indoor CO2 observations; outdoor PM2.5 is not a substitute for indoor measurements. This proposal is not a medical diagnostic tool or a claim of regulatory compliance.

## Proposed Evaluation

Experiments have not been run. The planned protocol includes:

- Chronological forecasting evaluation against persistence and statistical baselines, with LightGBM/LSTM as candidates.
- A comparison of current-threshold alerts and forecast-based warnings using a declared 30-minute horizon.
- Paired simulation of a rule-based policy and the proposed integrated policy under shared inputs and constraints.
- RAG and IAQ-rule ablations that retain hard safety interlocks in every test arm.
- Forecast accuracy, warning, policy, evidence-grounding, resilience, and—if approved—usability measures.

The **30-minute warning lead-time** and **40% exceedance-duration reduction** values are proposed targets, not achieved results. Data access, thresholds, simulator parameters, and applicable site rules must be verified before an experiment can be interpreted.

## Current Repository Status

This repository currently contains proposal and OpenSpec documentation, not an executable AirGuard application. Sensor/site availability, indoor datasets, reference measurements, simulator access, current legal applicability, and selected data-service terms remain unverified. Candidate software technologies have not been selected or installed as an application stack.

There are no application installation or run commands at this stage.

## Documentation

- [System design and requirements](DESIGN.md) — authoritative proposed architecture, requirements, safety boundaries, and evaluation plan.
- [Research proposal](proposal.md) — A–F abstract, G–L Introduction, Related Work, research questions, proposed scheme, and evaluation protocol.
- [OpenSpec change](openspec/changes/airguard-proposal-refactor/) — seven capability specifications and the proposal-refinement artifacts.

## Evidence Policy

Literature claims are limited to the source material identified in [proposal.md](proposal.md). Peer-reviewed studies, preprints, reviews, and inaccessible full text are distinguished there. No AirGuard performance, health, energy-saving, or control result should be inferred from proposed targets or results reported by other studies.
