# Design

## Context

See `proposal.md` for the motivation and scope. The repository has a proposal-baseline `DESIGN.md`, but no application implementation or existing capability specs. Its architecture, requirements, candidate technologies, targets, and evaluation plan are proposed, not evidence that a system has been implemented or evaluated. The specifications in this change express intended system behavior; they do not assert that behavior already exists.

The change translates the source design into a course-proposal structure and testable capability boundaries. The root `DESIGN.md` remains authoritative and unchanged.

The assignment-facing draft is now in the repository-root [proposal.md](../../../proposal.md). It contains the six A–F abstract sections, the G–L Introduction progression, a balanced AI/domain Related Work section, the five mindsets, three controversies, ten research questions, planned experiments, and a qualified references/source-status record.

## Goals / Non-Goals

**Goals:**

- Produce a research proposal organized around the DIC3 A–F abstract sections.
- Frame the Introduction and Related Work as research narrative rather than a list of system modules.
- Make domain reasoning, controversies, research questions, evaluation criteria, and evidence status explicit.
- Preserve the source design's safety, traceability, uncertainty, and implementation-status boundaries.
- Provide behavioral specifications derived from the source FR and EV requirements.

**Non-Goals:**

- Implement or claim implementation of ingestion, forecasting, RAG, agent, dashboard, or actuation features.
- Claim that a forecast, intervention, sensor correction, or control policy has been validated.
- Select a final technology stack, assert regulatory applicability, or certify any threshold.
- Add fabricated literature references or use the source's unverified numerical claims as results.
- Change `DESIGN.md` or perform physical HVAC/device control.

## Decisions

### Proposal structure

The abstract will use the exact requested A–F labels:

| Section | Proposal content |
|---|---|
| **A. Attention Getter** | Establish indoor air as a dynamic, time-dependent monitoring and decision problem. Treat this as the proposal's motivation, not a measured finding. |
| **B. Challenge** | Explain the design problem: changing indoor sources, outdoor conditions, occupancy and ventilation; imperfect low-cost sensors; and the risk of confusing indoor and outdoor standards or proxies. |
| **C. Cure** | Propose AirGuard AI as an evidence-grounded advisory workflow linking observations, quality checks, a short-horizon forecast, deterministic rules, retrieved domain evidence, and a human-readable recommendation. |
| **D. Development** | Describe the proposed eight-module architecture, data contracts, safety boundaries, and intended implementation sequence. All technology choices remain candidates until validated against available hardware and data. |
| **E. Experiments** | Specify planned time-based evaluation, baselines, metrics, and failure cases. Treat the approximately three-month indoor dataset and 70/15/15 chronological split as plans contingent on data access. |
| **F. Expected Findings** | State hypotheses to test, including whether the integrated workflow can improve warning lead time and whether simulated policies can reduce exceedance duration under comparable conditions. Explicitly say that outcomes are unknown; the 30-minute and 40% values are targets, not findings. |

### Refactored Introduction

Use the DIC3 G–L progression to move from the general indoor-air monitoring problem to the study contribution. The completed draft is in the repository-root [proposal.md](../../../proposal.md); citation-specific claims are limited to verified metadata or source abstracts and are qualified where full text was unavailable.

1. **General (G):** Indoor air-quality decisions depend on observations that change with pollutant sources, occupancy, ventilation, and outdoor conditions. A monitoring proposal therefore needs to connect measurements to their time, location, units, and quality rather than treating a displayed value as context-free.
2. **However (H):** A threshold notification alone does not establish which response is appropriate. The proposed system must account for sensor freshness and uncertainty, pollutant-specific behavior, outdoor-air trade-offs, applicable averaging periods, and the capabilities of the installed equipment.
3. **In literature (I):** Position the study against two bodies of work to be reviewed: data-driven air-quality forecasting and AI/agent-based decision support; and indoor-air domain work on pollutant measurement, ventilation, filtration, exposure, and standards. Do not state comparative literature findings until relevant papers and their methods are verified and cited.
4. **Judgment (J):** This proposal treats prediction and generated language as decision support, not as authority. Numeric risk classification belongs to versioned deterministic rules; retrieval supplies attributable context; and actuation remains advisory or simulated unless separately authorized and made safe.
5. **Keypoint (K):** The study asks whether a traceable IoT-to-recommendation workflow can provide useful short-horizon warnings and explainable, safety-constrained actions without overstating sensor evidence or evaluation results.
6. **Organization (L):** The proposal introduces related AI and domain work, defines the system and safety design, formulates ten research questions, and presents an evaluation protocol with explicit baselines, metrics, limitations, and evidence status.

### Refactored Related Work

Organize Related Work by the evidence needed to answer the research questions, balancing AI methods and domain constraints. The repository-root [proposal.md](../../../proposal.md) implements this framing. Preprint statements are attributed to the authors, metadata-only records carry no performance claims, and unavailable full text/regulatory materials are recorded as verification gaps.

| Theme | AI / technical focus | Air-quality domain focus | Comparison questions |
|---|---|---|---|
| Time-series sensing and forecasting | Persistence and statistical baselines; candidate LightGBM and LSTM models; input windows, forecast horizons, uncertainty | Indoor PM2.5 as the initial target; CO2 only with indoor measurements; sampling, humidity sensitivity, drift, and indoor/outdoor differences | Which horizon and target are supported by the available data? Are splits chronological and transformations fitted without leakage? |
| Data quality and calibration | Missingness, anomaly detection, data alignment, calibration methods | Sensor type, reference co-location, humidity effects, pollutant-specific response, event-versus-fault ambiguity | What evidence validates correction, and how are suspect values prevented from becoming ground truth? |
| Knowledge-grounded decision support | Retrieval-augmented generation, tool use, structured evidence, deterministic rules | Indoor versus outdoor standards, averaging times, applicability, device manuals, ventilation and filtration constraints | Can every rule and recommendation be traced to an applicable, versioned source? What must remain deterministic? |
| Agent safety and actuation | Planning, permissions, tool boundaries, human approval, auditability | HVAC/device limits, interlocks, safe fallback, occupant comfort, outdoor pollution | Which actions can be simulated or advised, and what independent safeguards would be required before physical control? |
| Evaluation and usability | Chronological model evaluation, ablation/comparator design, explanation and task measures | Exposure proxies, exceedance duration, energy proxies, false alarms, site conditions | Are outcomes comparable under shared conditions and reported without implying clinical or regulatory conclusions? |

The source register in the repository-root [proposal.md](../../../proposal.md) records checked records and unresolved gaps. Candidate sources listed in `DESIGN.md` (including named arXiv identifiers, standards, and data services) must still be checked against their original full text, scope, version, and terms before final submission. If a source cannot be verified, omit it or mark the gap; do not infer a finding or citation.

### Five core domain mindsets

1. **Mass balance:** Concentration reflects generation, transport, ventilation, and removal. The single-zone equation in `DESIGN.md` is a conceptual model only; pollutant-specific parameters and site measurements are needed before quantitative claims.
2. **Exposure over time:** A snapshot and an averaged or cumulative exposure answer different questions. Every threshold comparison must retain its pollutant, averaging period, site scope, and evidence; no health conclusion follows from an unvalidated proxy.
3. **Sensor drift and measurement limits:** A reading may reflect environmental change or sensor fault, humidity influence, or drift. Preserve raw readings, quality flags, calibration provenance, and uncertainty; do not treat an unvalidated correction as truth.
4. **Airflow and pressure:** Ventilation changes pollutant transport and can interact with pressure and building configuration. Do not claim ACH, pressure control, or airflow direction without the required measurements and equipment evidence.
5. **Health, energy, and comfort trade-offs:** A candidate action may reduce one pollutant while increasing energy use, discomfort, switching, or another pollutant exposure. Compare trade-offs under explicit constraints; do not collapse them into an unsupported single score.

### Three major controversies and trade-offs

| Controversy | Competing considerations | Proposal treatment |
|---|---|---|
| **Filtration vs. ventilation** | Filtration can address particles, while ventilation affects CO2 and other indoor conditions; outdoor pollution and actual HVAC capability constrain added outside air. | Compare only feasible, pollutant-aware candidates. Avoid the false choice of universally maximizing filtration or ventilation, and do not assume particle filtration removes CO2. |
| **Low-cost sensor credibility** | Low-cost sensors enable denser monitoring but may drift, respond to humidity, or differ from reference instruments. | Keep provenance and quality flags; evaluate against appropriate co-located reference data before using calibration claims. Outdoor stations are context, not indoor ground truth. |
| **Autonomous action vs. human oversight** | Automation may reduce response delay, but model, data, retrieval, and device errors can create unsafe actions. Human review adds control but may add latency. | Keep advisory as the default and simulation as the MVP action mode. Treat physical actuation as a separate future scope with authorization, interlocks, device validation, and human approval as applicable. |

### Ten deep research questions

1. **What evidence is sufficient** to distinguish a real pollution event from sensor drift, humidity interference, or a sensor fault?
2. **How sensitive is a 30-minute forecast** to sampling frequency, missingness, input-window choice, and pollutant-specific dynamics?
3. **When does indoor/outdoor context help or mislead** an indoor forecast, given station distance and microenvironment differences?
4. **What validation design is required** before a dynamic calibration method can improve accuracy without leakage or masking real events?
5. **How should averaging period and site scope affect risk interpretation** when an available standard or guideline does not directly match the indoor decision?
6. **What-if outdoor PM2.5 is elevated while indoor CO2 rises:** which feasible actions best balance pollutant exposure, ventilation, energy, and comfort in a common simulation?
7. **When should the system abstain** from forecasting or recommending action because input quality, model uncertainty, or retrieved evidence is inadequate?
8. **How much does sourced retrieval add** over deterministic rules alone, and how can unsupported citations or inapplicable device guidance be detected?
9. **Which actions require human approval or must remain unavailable**, given device capabilities, interlocks, permissions, and consequences of failure?
10. **What evidence would demonstrate a real benefit** in warning lead time or exceedance duration, and how can false alarms, site variation, and simulator limitations be separated from that benefit?

### Specification decomposition and traceability

The seven delta specifications are new capabilities because the repository has no existing capability specifications. They convert the design's FR-001–FR-014 and EV-01–EV-06 into externally observable requirements and scenarios:

| Capability spec | Primary source mapping |
|---|---|
| `sensor-ingestion` | FR-001–FR-003 |
| `data-quality` | FR-004, FR-013 and data-quality constraints |
| `air-quality-forecasting` | FR-005, FR-014, forecasting protocol |
| `domain-rules-and-rag` | FR-006–FR-008 and standards governance |
| `agent-planning-and-safety` | FR-009–FR-010 and safety policy |
| `dashboard-and-audit` | FR-011–FR-012 |
| `evaluation-protocol` | FR-014 and EV-01–EV-06 |

The cross-module component responsibilities and sequence remain in the root `DESIGN.md`. These delta specs define observable behavior, not a new implementation stack.

### Evidence and implementation status

Use three explicit evidence categories in the resulting proposal:

- **Existing in this repository:** the root `DESIGN.md` and this change's planning/specification artifacts.
- **Proposed, not implemented:** every AirGuard software component, data integration, forecast, rule catalog, RAG corpus, dashboard, audit store, and simulation behavior described by the design.
- **Planned targets or hypotheses:** at least 30 minutes warning lead time, at least 40% reduction in exceedance duration, and the proposed evaluation outcomes. These remain unverified until the protocol is run and results are reported.

No values such as the unverified control accuracy, energy savings, or avoidance rates in the source draft may be presented as AirGuard findings. The proposal cites source records only with explicit evidence limits; no citations or results are fabricated.

### Source verification status

The proposal's reference list records verified metadata for the cited journal/conference records, arXiv preprints, and WHO publications, together with the material actually inspected. Official documentation pages checked include the [OpenAQ API documentation](https://docs.openaq.org/), [Taiwan MOENV data portal](https://data.moenv.gov.tw/), [LASS/AirBox portal](https://lass-net.org/), and [LASS DCF status page](https://pm25.lass-net.org/DCF/). These page checks do not establish that a specific dataset, API term, calibration result, or sensor is suitable for the project. The exact current Taiwan legal provisions and indoor applicability, ASHRAE edition/clauses, selected data terms, and full texts not accessed remain unresolved and are not represented as verified.

## Risks / Trade-offs

- **Unavailable indoor data** → State data requirements and contingencies; do not substitute outdoor values as indoor truth or claim indoor validation without indoor observations.
- **Unverified sources and regulations** → Keep literature claims provisional; verify primary sources, versions, applicability, and licensing before citing or encoding rules.
- **Ambiguous domain parameters** → Treat the physical model and objective weights as conceptual until measurements and sensitivity analyses justify values.
- **Targets mistaken for outcomes** → Label targets, hypotheses, simulations, and measured results separately in the proposal and final report.
- **Overweighting AI novelty** → Retain pollutant, ventilation, measurement, and standards constraints as core Related Work and design concerns.
- **Overclaiming control safety** → Keep physical control outside MVP and require separate evidence and authorization before any future scope includes it.

## Migration Plan

1. Review the repository-root `proposal.md` and capability deltas against the unchanged root `DESIGN.md`.
2. Verify any additional citations, legal provisions, data terms, or site-specific assumptions before final DIC3 submission; preserve unresolved items as explicit limitations.
3. Validate the seven capability specifications and keep all implementation tasks pending until their behavior is implemented and tested in a separately authorized application-code phase.
4. Do not archive this change until the user requests it and its review requirements are satisfied. No application deployment or rollback is part of this documentation refactor.

## Open Questions

- Which indoor site and sensor streams can the project actually access, and at what sampling intervals?
- Which original, verifiable papers and standards will be selected for the final Related Work section?
- Can reference measurements and an appropriately controlled simulator be made available for the proposed evaluation?
