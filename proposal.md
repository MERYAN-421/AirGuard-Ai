# AirGuard AI: A Proactive Indoor Air-Quality Protection Agent

> **Document status:** Research proposal; proposed system, not implemented. No AirGuard experiment has been run.
> **Source of system requirements:** The repository's `DESIGN.md`, which remains unchanged.
> **Evidence boundary:** The targets below are plans, not findings. Literature statements are attributed to the cited source and limited to material checked for this draft.

## Abstract

### A. Attention Getter

Indoor-air management is a time-dependent decision problem: measurements, pollutant sources, occupancy, ventilation, and outdoor conditions may change on different schedules. A useful system should therefore do more than display a reading; it should preserve when and where a measurement was made, whether it is trustworthy, and what evidence supports a proposed response.

### B. Challenge

An alert based on a single threshold does not by itself identify an appropriate action. Sensor drift, missing or delayed readings, pollutant-specific behavior, indoor/outdoor differences, and standards with different scopes or averaging periods can all limit interpretation. Acting on an uncertain estimate may be as problematic as missing an emerging condition.

### C. Cure

This proposal introduces AirGuard AI, a planned indoor-air-quality decision-support system that connects IoT observations, data-quality checks, short-horizon forecasting, versioned deterministic rules, domain-document retrieval, and explainable recommendations. A language model may organize evidence and candidate actions, but it will not determine numeric thresholds or bypass safety rules.

### D. Development

The proposed system follows the eight-module architecture in `DESIGN.md`: context and UI; input ingestion; planner and agent; data acquisition; analytics; domain knowledge and rules; decision and actuation; and output and audit. The MVP boundary is monitoring, forecasting, explanation, and simulated device commands. Physical HVAC control is outside scope. Candidate technologies and integrations remain subject to hardware, data-access, and source verification.

### E. Experiments

Evaluation is planned with indoor observations if they become available, matched outdoor context where appropriate, chronological train/validation/test splits, and declared persistence and statistical baselines. Forecast metrics will include MAE and RMSE; warning evaluation will define event onset, lead time, precision, recall, and false-alarm rate. Policy comparisons will use a shared simulation environment and report exceedance duration and energy proxies. None of these experiments has been conducted.

### F. Expected Findings

The study will test whether a provenance-aware workflow can provide useful warnings and whether simulated, constrained recommendations can improve on declared baselines under comparable conditions. The design's target of at least 30 minutes' warning lead time and at least 40% lower exceedance duration are hypotheses/targets only. Outcomes may be neutral or negative; no result is asserted in this proposal.

## 1. Introduction

### General (G)

Indoor-air decisions depend on observations in a particular room, at a particular time, for a particular pollutant. AirGuard's design accordingly treats site, sensor, timestamp, metric, unit, source, and quality status as part of an observation rather than optional display details. This is a proposed data contract, not an implemented capability.

### However (H)

A measured or predicted concentration does not alone identify the right intervention. Low-cost sensor observations can have cross-sensitivities and can be affected by external conditions; a review by Concas et al. describes those calibration challenges for outdoor air-quality monitoring [1]. Indoor and outdoor evidence also have different scopes. The WHO's 2010 indoor-air guideline addresses selected pollutants and indoor exposure pathways, while its 2021 global air-quality guideline addresses specified ambient pollutants [4, 5]. These documents are not interchangeable threshold catalogs.

### In literature (I)

The relevant research spans two connected areas: computational methods for forecasting and agent-based decision support, and domain work on indoor pollutants, sensing, ventilation, and applicable standards. A 2026 arXiv review by Neubauer et al. examines LLM use in HVAC operations and reports that its reviewed corpus is weighted toward building-energy modeling, with limited pilot-level evidence and no reported sustained operational deployments in that corpus [2]. He and Jazizadeh describe a context-aware LLM-based building-energy-management framework and prototype assessment [3]. These works motivate bounded, human-centered assistance, but neither establishes that an AirGuard indoor-pollutant controller will be accurate or safe. The cited arXiv items are preprints; their claims must be assessed against their full methods and publication status before they are treated as settled evidence.

### Judgment (J)

This proposal positions AI as a support layer, not an authority over measurements, regulations, or equipment. Numeric risk classification will be assigned by deterministic, versioned rules; retrieval will provide attributable context; and generated text will explain options without inventing evidence. Advisory behavior is the default, simulation is the MVP action mode, and any physical actuation would require a separately authorized safety design.

### Keypoint (K)

The research question is whether a traceable workflow from indoor sensing to short-horizon analysis and constrained recommendations can make warnings more useful without overstating data quality, confusing indoor and outdoor standards, or claiming unmeasured benefits.

### Organization (L)

The remainder defines related work and its evidence boundaries, presents AirGuard's proposed architecture and safety policy, identifies domain mindsets and research controversies, lists ten research questions, and specifies a reproducible evaluation plan. The final section states hypotheses and limitations rather than results.

## 2. Related Work

### 2.1 AI and data methods

AirGuard's design proposes persistence and simple statistical baselines before comparing candidate LightGBM and LSTM forecasters. This ordering makes it possible to measure whether added model complexity contributes beyond a reproducible reference. The 2024 IEEE conference record by Singh, Islam, and Dinh is titled *Forecasting Indoor Air Quality Using Machine Learning Models* [6]. Its bibliographic metadata is verified, but the full paper and abstract were not available during this review; therefore, this proposal does not attribute a model, dataset, or performance result to it.

For agent systems, Neubauer et al.'s 2026 review preprint surveys LLM applications in HVAC operations and explicitly considers deployment readiness and the responsibility boundary between LLMs and physical HVAC decisions [2]. Its abstract reports 66 peer-reviewed studies in the review corpus and describes only a small subset as pilot-level; those are the review authors' reported classifications, not independently re-evaluated evidence here. He and Jazizadeh's 2025 preprint describes perception, central-control, and action/user-interaction functions for a building-energy-management agent [3]. Its energy-management setting is related but is not evidence for pollutant forecasting or indoor-air control performance.

These sources support a research comparison rather than an assumption that an LLM should control devices. AirGuard will compare forecast models against non-LLM numerical baselines and keep risk thresholds outside free-form generation. The proposed retrieval and agent contribution is limited to gathering evidence, structuring candidate actions, and explaining limitations.

### 2.2 Indoor-air domain knowledge

Domain evidence changes how the computational results should be interpreted. Concas et al.'s survey describes low-cost outdoor sensor cross-sensitivities, external influences, and degradation over time, and reviews machine-learning calibration approaches [1]. Its stated setting is outdoor monitoring, so its findings cannot automatically be generalized to every indoor sensor or pollutant. AirGuard therefore requires sensor-specific provenance and reference validation before describing any correction as accurate.

For authoritative context, the WHO's *Guidelines for Indoor Air Quality: Selected Pollutants* (2010) covers selected chemicals, indoor sources, exposure pathways, indoor/outdoor relationships, health evaluations, and guideline material [4]. The WHO's *Global Air Quality Guidelines* (2021) covers listed ambient pollutants [5]. Their stated scope supports keeping indoor guidance and ambient-air guidance distinct; it does not supply a site-specific AirGuard threshold. The applicable Taiwan requirements and any ASHRAE provisions must still be checked in their official, current versions for the actual site before use.

| Literature area | AI / technical question | Domain question | AirGuard treatment |
|---|---|---|---|
| Forecasting | Do LightGBM/LSTM candidates improve on persistence and statistical baselines at a declared horizon? | Are target, sampling interval, and indoor ground truth available? | Compare chronologically; disclose unsupported targets. |
| Sensor quality | Can calibration or anomaly methods improve usable data? | Does the method match this sensor, pollutant, humidity range, and reference instrument? | Preserve raw values and validate corrections before use. |
| Retrieval and rules | Does retrieval improve evidence-grounded explanations beyond deterministic rules? | Is a document current and applicable to the pollutant, averaging period, site, and device? | Keep retrieved citations separate from numeric rule authority. |
| Agent and control | Can a planner rank candidate actions reliably under constraints? | What equipment limits, outdoor conditions, and human approvals apply? | Advisory/simulation only in MVP; no autonomous physical actuation. |
| Evaluation | Are gains reproducible and meaningful against a fair comparator? | Do metrics capture false alarms, exceedance duration, energy proxies, and site limits? | Predefine metrics and label simulated results as simulated. |

### 2.3 Source verification and citation limits

The cited records were checked against their originating or registration sources: arXiv records for [2] and [3], Crossref metadata for [1] and [6], and WHO publication pages for [4] and [5]. ACM and MDPI full-text pages were inaccessible during this review; [1] is used only for claims present in its Crossref abstract, and [4] is used only for scope stated on the WHO publication page. The full text of [2] and [3] was not independently reviewed. IEEE metadata for [6] confirms the publication record and title only.

The official OpenAQ documentation provides API usage and terms links; the MOENV data portal and LASS/AirBox portal are reachable. The LASS DCF status page identifies PM2.5, temperature, relative humidity, and hour as features and lists several candidate regression methods. These checks establish that documentation/pages exist, not that any specific endpoint, dataset, API terms, update cadence, calibration performance, or site data is suitable for this project. The MOENV API request tested without an API key was rejected as expected; authenticated access and dataset-specific conditions remain unverified. The ASHRAE standards page returned an access error in this review, so no ASHRAE clause, edition, or threshold is cited. The exact applicable Taiwan indoor-air rules and their current scope remain to be verified against official legal text.

## 3. Proposed System and Development

AirGuard's proposed flow is: ingest indoor IoT and selected outdoor observations; preserve raw values and normalize units/time; mark quality and freshness; forecast an available indoor target over the proposed 30-minute horizon; evaluate versioned deterministic rules; retrieve applicable documents; generate evidence-linked action candidates; apply permission/device/safety checks; and present an advisory or simulated result with an audit record. This preserves the eight modules and data flow specified in `DESIGN.md`.

The first target is indoor PM2.5. CO2 forecasting depends on real indoor CO2 observations; outdoor PM2.5 is not a substitute. The single-zone mass-balance equation in the design is a conceptual model until site volume, pollutant generation, ventilation, and removal parameters are measured. Candidate Python, API, database, model, retrieval, and deployment technologies in `DESIGN.md` remain candidate selections, not installed or implemented components.

The MVP is safe-by-default: advisory recommendations and simulated device commands only. Any future physical mode would require explicit authorization, device-specific constraints, permissions, interlocks, timeout handling, and human approval as applicable. Missing or stale evidence must lead to an explicit unknown/limited status rather than a compliance claim or hazardous action.

## 4. Core Domain Mindsets

1. **Mass balance:** A concentration depends on generation, ventilation/transport, and removal. The design's one-zone equation is a conceptual starting point, not a calibrated prediction of a real room.
2. **Exposure over time:** A snapshot, an averaging period, and a cumulative-duration metric answer different questions. Every rule and experiment must retain the pollutant, units, averaging window, and site scope.
3. **Sensor drift and measurement limits:** A reading may change because the environment changed or because the sensor is affected by drift, cross-sensitivity, humidity, or failure. Keep raw values, quality flags, calibration provenance, and uncertainty.
4. **Airflow and pressure:** Ventilation changes transport and may interact with pressure and building layout. Do not claim air-change rate, pressure control, or airflow direction without corresponding measurements and equipment evidence.
5. **Health, energy, and comfort trade-offs:** A proposed action can have different effects across pollutants, energy use, switching, and comfort. Compare explicit measures and constraints; do not present an unvalidated weighted objective as an optimized policy.

## 5. Research Controversies and Trade-offs

1. **Filtration versus ventilation:** Filtration and ventilation address different parts of the problem, and outside-air quality and actual HVAC capability constrain options. The experiment should compare only physically feasible candidates and must not assume particle filtration removes CO2 or that reduced outside air is always safe.
2. **Low-cost sensor credibility:** Low-cost sensors can expand spatial coverage but can also be affected by cross-sensitivity, external conditions, and calibration drift [1]. AirGuard must treat external monitors as context, not indoor ground truth, and test any calibration against suitable reference measurements.
3. **Autonomous action versus human oversight:** Automation may reduce response delay, while model, sensor, retrieval, and device faults create action risks. The proposed compromise is an explainable advisory system with simulation in the MVP and a separate safety review before physical control.

## 6. Deep Research Questions

1. What evidence is sufficient to distinguish a real pollution event from sensor drift, humidity influence, or sensor failure?
2. How sensitive is a 30-minute forecast to sampling interval, missingness, input-window length, and pollutant-specific dynamics?
3. When does outdoor context improve an indoor forecast, and when does station distance or microenvironment mismatch make it misleading?
4. What reference-measurement design is needed to validate a dynamic calibration method without data leakage or masking genuine events?
5. How should pollutant, averaging period, site scope, and source version affect risk interpretation when an available standard does not directly match the indoor decision?
6. If outdoor PM2.5 is elevated while indoor CO2 rises, which feasible actions best balance pollutant exposure, ventilation, energy, and comfort in a common simulation?
7. When should the system abstain from forecasting or recommending action because input quality, model uncertainty, or retrieved evidence is insufficient?
8. What does sourced retrieval add beyond deterministic rules, and how can unsupported citations or inapplicable device guidance be detected?
9. Which actions require human approval or must remain unavailable given device capabilities, interlocks, permissions, and consequences of failure?
10. What evidence would establish a real benefit in warning lead time or exceedance duration, while separating false alarms, site variation, and simulator limitations?

## 7. Experimental Plan

### Data and split

The design proposes approximately three months of indoor logs, corresponding outdoor context, and a chronological 70/15/15 train/validation/test split. These are plans contingent on data access, not claims that a dataset exists. The team must record sensor type, site, sampling interval, timestamps, units, quality, provenance, reference co-location, and missingness. Preprocessing, calibration, feature engineering, and model selection must avoid using future test data.

### Baselines and measures

- **Forecasting:** persistence and simple statistical baselines first; LightGBM and LSTM are candidates. Report MAE and RMSE, with separate results for ordinary periods, events, and missing-source conditions.
- **Warnings:** compare current-threshold and forecast-based warning approaches; define event onset and calculate lead time, precision, recall, and false-alarm rate.
- **Decision policy:** compare a rule-based baseline and proposed policy in the same simulator under the same external conditions. Report exceedance duration, exposure proxies, energy proxies, and switching counts as simulated measures.
- **Grounding and safety:** measure source-support and rule-applicability checks, unauthorized-command outcomes, and behavior under stale data, missing values, drift, abnormal readings, and provider outage.
- **Usability:** if participants and approval are available, compare a simple rule alert with an explained recommendation using task completion, response time, and a declared rating instrument. No human-subject evaluation is claimed or authorized by this proposal.

All experiment settings must record data window, split, random seed where relevant, model/configuration version, code version, and metric definitions. The proposal targets at least 30 minutes of warning lead time and at least 40% lower exceedance duration. They remain target values until validated under a specified protocol; neither should be reported as achieved without measured results.

## 8. Expected Findings and Limitations

The expected contribution is a testable design for joining time-series analysis with domain evidence and safety constraints, plus a protocol for comparing its components. The experiments may find that the proposed forecast does not beat persistence, that warning gains are offset by false alarms, or that available data cannot support a reliable intervention comparison. Such outcomes would narrow the claim rather than be hidden.

No finding about AirGuard's accuracy, energy savings, health impact, compliance, or physical control is available. The project currently has no verified indoor sensor inventory, site history, reference co-location plan, simulator result, or physical-device authorization in this repository. No medical diagnosis, statutory compliance, or broad multi-site generalization is claimed.

## 9. Implementation and Evidence Status

- **Existing in the repository:** the system-design baseline and planning/research documentation. No executable AirGuard application or verified site dataset was found in the repository.
- **Proposed, not implemented:** every AirGuard software module, integration, forecast, rule catalog, retrieval corpus, dashboard, audit store, and simulation feature described here.
- **Measured results:** none. Forecast, warning, usability, and policy experiments have not been run. The 30-minute and 40% figures are targets only.

## References

[1] F. Concas, J. Mineraud, E. Lagerspetz, S. Varjonen, X. Liu, K. Puolamäki, P. Nurmi, and S. Tarkoma, “Low-Cost Outdoor Air Quality Monitoring and Sensor Calibration,” *ACM Transactions on Sensor Networks*, vol. 17, no. 2, pp. 1–44, 2021. https://doi.org/10.1145/3446005. Bibliographic metadata and abstract checked via Crossref; publisher full text was inaccessible during this review.

[2] A. Neubauer, T. Hong, H. Li, M. Yu, A. Darbandi, Y. Fürst, and M. Kriegel, “Large Language Models for HVAC Operations in Building Energy Systems: A Critical Review of Methods, Applications, and Deployment Readiness,” arXiv:2609.05314, version 1, submitted September 4, 2026. https://arxiv.org/abs/2609.05314. Preprint; title, authors, date, and abstract checked on arXiv. Full text and publication status were not independently reviewed.

[3] T. He and F. Jazizadeh, “Context-aware LLM-based AI Agents for Human-centered Energy Management Systems in Smart Buildings,” arXiv:2512.25055, version 1, submitted December 31, 2025. https://arxiv.org/abs/2512.25055. Preprint; title, authors, date, and abstract checked on arXiv. Full text and publication status were not independently reviewed.

[4] World Health Organization, *WHO Guidelines for Indoor Air Quality: Selected Pollutants*, 2010. https://www.who.int/publications/i/item/9789289002134. Publication scope checked on the WHO record.

[5] World Health Organization, *WHO Global Air Quality Guidelines: Particulate Matter (PM2.5 and PM10), Ozone, Nitrogen Dioxide, Sulfur Dioxide and Carbon Monoxide*, 2021. https://www.who.int/publications/i/item/9789240034228. Publication scope checked on the WHO record; this is ambient-air guidance, not an indoor rule catalog.

[6] A. Singh, M. Islam, and N. Dinh, “Forecasting Indoor Air Quality Using Machine Learning Models,” in *2024 IEEE International Conference on Consumer Electronics (ICCE)*, 2024, pp. 1–6. https://doi.org/10.1109/ICCE59016.2024.10444387. Crossref metadata checked; full text and abstract were unavailable during this review, so no finding is attributed to it.

## Source Verification Notes

- **Verified source records used above:** [1]–[6] as qualified in their entries; official WHO publication pages; the [OpenAQ API documentation](https://docs.openaq.org/using-the-api/quick-start); the [Taiwan MOENV data portal](https://data.moenv.gov.tw/); the [LASS/AirBox portal](https://lass-net.org/); and the [LASS DCF status page](https://pm25.lass-net.org/DCF/).
- **Not verified for project use:** the exact current Taiwan legal provisions and indoor-air applicability; current [ASHRAE standards](https://www.ashrae.org/technical-resources/standards-and-guidelines) 62.1 edition/clauses (the official page was inaccessible); MOENV API key/terms and dataset-specific availability; OpenAQ third-party license/terms for a selected station; LASS data terms and whether its DCF methods apply to this project's sensor; and full text/methods for [2], [3], and [6].
- **Unverified claims from the source design:** the draft's numerical claims about control accuracy, energy savings, and secondary-pollution avoidance are excluded as AirGuard findings. They require source-level verification and are not used in this proposal.
