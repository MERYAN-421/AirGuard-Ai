# Proposal

## Why

The current `DESIGN.md` is a system-design baseline, not a research proposal organized for the DIC3 assignment. It contains the intended architecture and evaluation plan, but the proposal needs a coherent A–F abstract, a research-led Introduction and Related Work, and explicit domain mindsets, controversies, and research questions. This change turns the existing design into a testable, evidence-conscious proposal without changing application code or the source document.

## What Changes

- Reframe the research proposal into **A. Attention Getter, B. Challenge, C. Cure, D. Development, E. Experiments, and F. Expected Findings**.
- Refactor the Introduction around the DIC3 general-to-specific progression: context, gap, literature positioning, judgment, key point, and organization.
- Refactor Related Work to balance AI methods (forecasting and agent systems) with indoor-air-quality domain knowledge (pollutants, sensing, ventilation, standards, and safety).
- Explicitly discuss five domain mindsets: mass balance, exposure over time, sensor drift, airflow/pressure, and health/energy trade-offs.
- Analyze three controversies and their trade-offs: filtration versus ventilation, low-cost sensor reliability, and autonomous action versus human oversight.
- Formulate ten deep research questions covering assumptions, failure modes, trade-offs, and evidence requirements.
- Translate the functional and evaluation requirements in `DESIGN.md` into seven capability specifications with testable scenarios.
- Keep all claims aligned with the source document: distinguish proposed features and hypotheses from implemented features and measured results; do not invent findings or citations.

## Capabilities

### New Capabilities

- `sensor-ingestion`: Traceable ingestion and normalization of indoor sensor and outdoor-provider observations.
- `data-quality`: Explicit quality checks, freshness handling, and safe degradation for incomplete or unreliable observations.
- `air-quality-forecasting`: Reproducible short-horizon forecasts and experiment metadata.
- `domain-rules-and-rag`: Versioned deterministic risk rules and sourced domain-document retrieval.
- `agent-planning-and-safety`: Evidence-grounded recommendations constrained by permissions, device capabilities, and execution mode.
- `dashboard-and-audit`: Clear presentation of observations, forecasts, evidence, decisions, and audit history.
- `evaluation-protocol`: Reproducible evaluation of forecast, warning, decision, resilience, and usability behavior.

### Modified Capabilities

None. The repository has no existing OpenSpec capability specifications.

## Impact

- Adds planning artifacts under `openspec/changes/airguard-proposal-refactor/`.
- Uses the root `DESIGN.md` as the authoritative source; that file remains unchanged.
- Adds a research-proposal document and checked citation records; no application source code, APIs, dependencies, or experimental results are added or changed. The repository currently has no application implementation to describe as completed.
- The proposed architecture, technical stack, quantitative targets, and evaluation plan remain proposals until verified through implementation and experiments.
