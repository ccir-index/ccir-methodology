# CCIR Governance Framework v1.0
## Compute Credit Index Research — Index Governance and Legal Framework

**Version:** 1.0
**Effective Date:** March 10, 2026
**Supersedes:** None — initial public release
**Published by:** CCIR — Compute Credit Index Research
**Repository:** https://github.com/ccir-index/ccir-methodology
**Companion document:** CCIR Methodology v1.0
**Contact:** research@ccir.io
**Governance standard:** Designed with reference to the IOSCO Principles for Financial Benchmarks (July 2013)

---

## Table of Contents

1. [Definitions](#1-definitions)
2. [Administrator and Calculation Agent Framework](#2-administrator-and-calculation-agent-framework)
3. [Conflict of Interest Policy](#3-conflict-of-interest-policy)
4. [Governance Structure](#4-governance-structure)
5. [Change Control](#5-change-control)
6. [Restatements](#6-restatements)
7. [Fallback and Benchmark Replacement](#7-fallback-and-benchmark-replacement)
8. [Administrative Discretion](#8-administrative-discretion)
9. [Data Collection and Third Party Oversight](#9-data-collection-and-third-party-oversight)
10. [Operational Resilience and Business Continuity](#10-operational-resilience-and-business-continuity)
11. [Audit and Independent Review](#11-audit-and-independent-review)
12. [Complaints Procedure](#12-complaints-procedure)
13. [Regulatory Cooperation](#13-regulatory-cooperation)
14. [Intellectual Property and Licensing](#14-intellectual-property-and-licensing)
15. [Rating Agency Disclosure Statement](#15-rating-agency-disclosure-statement)
16. [Disclaimer](#16-disclaimer)
17. [Appendix A — Suggested Legal Citation Language](#appendix-a--suggested-legal-citation-language)
18. [Appendix B — Version History](#appendix-b--version-history)
19. [Appendix C — IOSCO Principles Alignment Summary](#appendix-c--iosco-principles-alignment-summary)

---

## 1. Definitions

The following terms have the meanings set forth below. Defined terms are capitalized throughout for interpretive consistency. Technical terms specific to index calculation (Observation, Qualifying Listing, Calculation Window, Confidence Flag, SHA-256 Hash) are defined in CCIR Methodology v1.0.

**"Administrator"** means CCIR — Compute Credit Index Research, the entity responsible for governance, oversight, and publication of the Indices.

**"Calculation Agent"** means the function responsible for mechanical computation of an Index in accordance with the CCIR Methodology, whether performed internally by the Administrator or delegated to a third party pursuant to Section 2.2.

**"Index"** or **"Indices"** means CRI-H100 and, as applicable, the companion indices (CRI-A100, CRI-H200, CRI-L40S, CRI-B200), each as defined in CCIR Methodology v1.0.

**"Material Methodology Change"** has the meaning set forth in Section 5.2.

**"Restatement"** means a correction to a previously published Index value pursuant to Section 6.

**"Fallback Value"** means a value determined pursuant to the fallback waterfall in Section 7.

**"Publication Date"** means each calendar day on which an Index is published pursuant to the daily publication schedule.

**"Business Day"** means any weekday other than a U.S. federal holiday.

**"Confidence Flag"** means the `high`, `medium`, or `low` disclosure applied to each Index publication, as defined in CCIR Methodology v1.0, Section 7.1.

**"Primary Data Source"** means the Vast.ai GPU rental marketplace API. CCIR also collects from Shadeform and RunPod as secondary sources, as described in Methodology §4.

**"Annual Benchmark Review"** means the annual assessment described in Section 4.3.

**"Monthly Source Registry Review"** means the operational review described in Section 9.4.

---

## 2. Administrator and Calculation Agent Framework

### 2.1 Administrator Responsibilities

The Administrator is responsible for:

- Maintaining and publishing the CCIR Methodology and this Governance Framework
- Governance oversight and version control of all CCIR documents
- Daily publication of Index values
- Maintaining the audit trail and archival data
- Disclosing conflicts of interest and Material Methodology Changes
- Oversight of data collection and third party data sources (Section 9)
- Commissioning independent review as appropriate
- Compliance with the provisions of this Governance Framework

The Administrator does not exercise discretion in routine Index calculation. All routine computation is governed by the mechanical rules in the CCIR Methodology.

### 2.2 Calculation Agent Function

The Calculation Agent applies the mechanical rules set forth in the CCIR Methodology. The Calculation Agent:

- Has no authority to override filters, data inclusion rules, or statistical procedures except as provided in Section 8
- Must document and log any technical failure or exception
- Must escalate any non-routine determination to the Administrator for review

If the Calculation Agent function is delegated to a third party, the delegation must be documented, the delegate must be subject to oversight controls, and the delegation must be publicly disclosed.

### 2.3 Separation of Functions

The Administrator and Calculation Agent functions shall be operationally separated to the extent practicable. Routine calculation shall not be subject to discretionary override by any person with a financial interest in the Index value.

### 2.4 Data Input Design

Each Index is calculated exclusively from publicly available data obtained through automated collection from designated Data Sources. No submissions from market participants are solicited or accepted. This design eliminates the category of conflicts of interest inherent in submission-based benchmarks — the governance challenge that prompted the IOSCO Principles for Financial Benchmarks following the LIBOR manipulation scandal.

The data input hierarchy for routine Index determination is:

1. **Automated API collection** per the published Methodology (no discretion exercised)
2. **Manifest Error Override** by the Administrator under Section 8.2 (discretion exercised, documented, and disclosed)
3. **Fallback provisions** per Section 7 (triggered by data unavailability, not discretion)

Expert judgment is not applied in routine Index determination.

---

## 3. Conflict of Interest Policy

### 3.1 Independence Commitment

The Administrator maintains policies designed to preserve the independence and integrity of the Indices, including:

- Separation between governance and any commercial activities that could benefit from a particular Index value
- Disclosure of material economic interests in GPU rental markets, GPU hardware markets, or GPU-backed financial instruments
- Recusal procedures for board members or personnel with conflicts

### 3.2 Prohibited Conduct

The following conduct is expressly prohibited for the Administrator, Calculation Agent, Oversight Board members, and any personnel involved in Index governance or calculation:

- Trading in GPU rental contracts, GPU-backed securities, or financial instruments whose value is directly linked to any Index value, based on advance knowledge of any Index publication
- Communicating any Index value or input data to any third party prior to publication
- Accepting compensation from any third party contingent on a particular Index level
- Accepting any instruction regarding the Index value from any party with a financial interest in that value

### 3.3 Independence from GPU Markets

CCIR does not operate a GPU rental marketplace, own GPU hardware inventory, hold GPU-backed loans, or provide GPU rental brokerage services. CCIR has no commercial interest in GPU rental prices, GPU hardware liquidation values, or the financial performance of any GPU rental marketplace operator.

This structural independence is the core governance feature that distinguishes an independent reference rate from a self-reported price. CCIR cannot benefit commercially from any particular Index level.

### 3.4 Disclosure Obligations

Any material conflict of interest identified by a board member or CCIR personnel must be disclosed to the Oversight Board chair (or, prior to formation of the Oversight Board, documented in writing in the CCIR governance record) within 5 Business Days of identification. CCIR will publicly disclose any material conflicts on an annual basis as part of the Annual Benchmark Review.

---

## 4. Governance Structure

### 4.1 Current Structure

CCIR is a Delaware LLC. In the initial period following first publication, governance functions are performed by the Administrator pending constitution of the Oversight Board. This is a transitional arrangement and is disclosed as such.

### 4.2 Oversight Board

CCIR commits to constituting an independent Oversight Board within 12 months of the date of first Index publication (target: March 2027).

The Oversight Board shall:

- Comprise 3 to 5 independent members with relevant expertise in structured finance, credit markets, data governance, or legal/regulatory compliance
- Meet at minimum quarterly
- Review the Annual Benchmark Review report and approve any Material Methodology Changes
- Review complaints escalated from the complaints procedure
- Appoint and oversee the independent auditor
- Have no commercial interest in GPU rental markets, GPU hardware, or GPU-backed financial products

### 4.3 Annual Benchmark Review

CCIR shall conduct an Annual Benchmark Review within 90 days of each anniversary of first publication. The review shall assess:

- Whether the Index continues to measure a genuine, active, and representative market
- Data sufficiency: observation counts, confidence flag history, and source diversity
- Whether the Methodology remains appropriate and whether changes are warranted
- Conflict of interest disclosures
- Complaints received and resolved
- Status of planned governance commitments (Oversight Board formation, independent audit)
- Summary of all Monthly Source Registry Reviews completed in the prior year, including any changes made and their classification

The Annual Benchmark Review shall be published on the CCIR GitHub repository.

---

## 5. Change Control

### 5.1 Versioning Policy

All changes to the Methodology or this Governance Framework are governed by the semantic versioning policy in [VERSIONING.md](./VERSIONING.md). In summary:

- **Patch versions** (x.x.N): Non-material clarifications, typographic corrections, cross-reference updates. No notice required.
- **Minor versions** (x.N.x): Non-material additions or expansions (e.g., adding a new companion index under an identical methodology). 14-day advance notice.
- **Major versions** (N.x.x): Material Methodology Changes. 60-day advance notice and 30-day stakeholder consultation.

### 5.2 Material Methodology Changes

The following changes constitute Material Methodology Changes requiring 60-day advance notice:

- Changes to the calculation method (e.g., replacing median-of-medians with mean)
- Changes to quality filter criteria (e.g., modifying the outlier removal threshold)
- Addition of a new data source as a **primary** source or removal of any source
- Changes to the Calculation Window length
- Changes to the outlier removal methodology
- Changes to the Confidence Flag thresholds
- Geographic scope changes

The following changes are **not** material:

- Addition of companion indices under an identical methodology
- Promotion of a companion index from internal collection to live publication
- Addition of secondary data sources that increase coverage without changing the primary source hierarchy
- Publication frequency changes that increase (not decrease) frequency
- Clarifications to existing provisions without substantive change
- Source registry updates identified through the Monthly Source Registry Review (e.g., GPU model nomenclature updates, new canonical name mappings)

### 5.3 Stakeholder Consultation

For Material Methodology Changes, CCIR shall publish a proposed change with a 30-day comment period prior to the 60-day advance notice period. Written responses must be submitted to research@ccir.io. CCIR shall publish a summary of comments received and CCIR's response as part of the change documentation.

### 5.4 Emergency Changes

In the event of a data source failure, API structural change, or other operational emergency that prevents publication under the current Methodology, CCIR may implement a temporary operational change to maintain publication continuity. Emergency changes must be:

- Documented and publicly disclosed within 1 Business Day
- Narrowly scoped to address the operational issue
- Assessed for materiality within 5 Business Days
- Followed by a formal change control process if the change is determined to be material

---

## 6. Restatements

### 6.1 No Routine Revision

The published Index series is **append-only**. Values are not revised in the ordinary course of business. Market movements, changes in data source coverage, or retrospective improvements to methodology do not constitute grounds for Restatement.

### 6.2 Manifest Error Standard

A Restatement is permitted only upon identification of a **Manifest Error**: an obvious, verifiable, and material error in the published value caused by:

- Data file corruption or transmission error
- API malfunction producing clearly anomalous input data
- Calculation script error producing a result inconsistent with the published Methodology
- Transcription or formatting error in the published output

A Manifest Error is distinguished from:
- A market movement that makes a published value appear incorrect in retrospect
- A data quality issue within the normal range of disclosed limitations
- A methodology judgment call that, in hindsight, could have been made differently

### 6.3 Restatement Procedure

Upon identification of a potential Manifest Error:

1. The Administrator shall document the error, the affected publication(s), and the corrected value
2. The Oversight Board (or, during the transitional period, a designated senior CCIR officer) shall review and approve the Restatement within 2 Business Days
3. A corrected value shall be published with a `[RESTATED]` label, the original value preserved in the record, and a public correction notice
4. The correction notice shall include: the original value, the restated value, the date of original publication, the date of restatement, and the reason for restatement
5. The correction notice shall be retained permanently in the public repository

---

## 7. Fallback and Benchmark Replacement

### 7.1 Fallback Waterfall

If the Index cannot be calculated or published on a scheduled Publication Date, the following waterfall applies in order:

**Level 1 — Delayed Publication.** If collection or calculation is delayed, CCIR shall publish as soon as practicable, with a notice of delay.

**Level 2 — Prior Value.** If Level 1 is not achievable within 24 hours, the most recently published Index value shall be deemed the Index value for the missed date, with a `low` confidence flag and a note identifying it as a carried-forward value.

**Level 3 — Interpolation.** If Level 2 applies for more than 5 consecutive Business Days, CCIR shall publish an interpolated value based on the most recent available data from any accessible Data Source, with a `low` confidence flag and full disclosure of the interpolation methodology.

**Level 4 — Benchmark Replacement.** If no Index value has been published within 30 calendar days, the Benchmark Replacement Waterfall in Section 7.2 applies.

### 7.2 Benchmark Replacement Waterfall

In the event of permanent cessation of Index publication:

1. **First replacement:** An index published by an independent administrator under a substantially similar methodology, as designated by CCIR on the cessation date
2. **Second replacement:** The most recently available hyperscaler on-demand rate for H100 SXM (per CRI-R), as an approximate measure of the relevant compute market
3. **Third replacement:** A rate agreed between the parties to the relevant credit agreement in good faith

CCIR commits to providing at least 90 days' advance notice of any planned cessation, to the extent practicable.

### 7.3 Transition Assistance

In the event of cessation, CCIR shall use commercially reasonable efforts to:

- Publish a transition notice identifying the recommended replacement
- Make all historical data available in the public repository for at least 3 years from cessation date
- Cooperate with any successor administrator in knowledge transfer

---

## 8. Administrative Discretion

### 8.1 Rules-Based Determination

CRI-H100 and all companion indices are determined by mechanical application of the rules in the CCIR Methodology. The Calculation Agent and Administrator do not exercise discretion in routine determination.

### 8.2 Manifest Error Override

The Administrator may override a calculated Index value only upon identification of a Manifest Error under Section 6. Any override must be documented, approved by a designated senior CCIR officer, disclosed publicly within 1 Business Day, and trigger the Restatement procedure in Section 6.3.

### 8.3 Limits on Administrative Discretion

The Administrator shall not:

- Apply expert judgment to substitute for or supplement the published Methodology in routine calculation
- Override a calculated value because it appears surprising or anomalous (absent a Manifest Error)
- Apply a different calculation methodology to any publication without completing the change control process in Section 5
- Suppress a publication because the Confidence Flag would be `low` or `medium`

---

## 9. Data Collection and Third Party Oversight

### 9.1 Data Source Inventory

As of Methodology v1.0:

| Source ID | Provider | Role | Authentication | Public URL |
|-----------|----------|------|---------------|------------|
| VASTAI_SPOT | Vast.ai | Primary | None required | console.vast.ai/api/v0/bundles |
| SHADEFORM_MKT | Shadeform | Secondary | Free API key | api.shadeform.ai |
| RUNPOD_SPOT | RunPod | Secondary | Free API key | api.runpod.io/graphql |

### 9.2 Collection Controls

The data collection pipeline:

- Runs on an automated hourly schedule without manual intervention in routine operation
- Archives each collection snapshot with a SHA-256 hash at the time of collection
- Logs all collection events, errors, and exceptions
- Alerts the Administrator of collection failures within 1 hour

### 9.3 Third Party Data Source Oversight

CCIR monitors each Data Source on an ongoing basis:

- **Monthly operational review:** Source registry, model nomenclature, and tier classification reviewed monthly per Section 9.4
- **Quarterly assessment:** API availability, schema stability, and observation count trends
- **Annual due diligence:** Review of each Data Source's operating status, known reliability issues, and continued suitability as a benchmark input
- **Trigger-event review:** Immediate assessment upon any API structural change, prolonged unavailability (>2 Business Days), or material decline in observation counts
- **Relationship disclosure:** CCIR has no commercial relationship with Vast.ai, Shadeform, or RunPod beyond API access. CCIR does not receive compensation from any data source for inclusion in the Index.

### 9.4 Monthly Source Registry Review

CCIR shall conduct a Monthly Source Registry Review within the first 10 calendar days of each month. The review is conducted by the Calculation Agent and documented in the governance record. Each review covers the following items:

**GPU Model Nomenclature**
Review any changes to model name strings returned by source APIs. GPU manufacturers and marketplace operators update product names, SKU designations, and variant nomenclature over time. Stale name mappings can silently exclude qualifying listings from the index. Review items include:
- New canonical name strings for covered models (H100 SXM, A100 SXM, H200, L40S, B200)
- New model variants requiring explicit inclusion or exclusion rules
- Changes to `gpu_name`, `gpu_model_raw`, or equivalent API fields across all sources

**Generation Migration Monitoring**
Assess the distribution of listing volume across GPU generations to detect generational transitions in progress. As newer generations (H200, B200) enter marketplace supply, H100 SXM listing depth may decline. Review items include:
- Month-over-month change in H100 SXM qualifying listing count per source
- Emergence of new GPU model families in source API data not currently captured in the model registry
- Changes in the ratio of H100 SXM to H200/B200 listings as a leading indicator of generational transition

**Tier Classification Accuracy**
Review the accuracy of the `instance_tier` classifications used in the CRI-R Companion Series. Marketplace operators periodically rename instance types, introduce new tiers, or discontinue existing ones. Review items include:
- Accuracy of spot / community / secure classification for RunPod
- Price-threshold validity for GCP tier classification (hpc_premium vs. commodity thresholds reviewed against current SKU pricing)
- Azure and AWS instance family keyword lists reviewed against current SKU catalogue
- Any new instance families introduced by providers that require classification

**API Schema and Endpoint Changes**
Confirm that collection pipelines are operating against current API specifications. Review items include:
- API endpoint availability and response schema for all three sources
- Any deprecation notices published by source providers
- Field name or data type changes that could silently corrupt collection output
- Rate limit or authentication changes

**Source Health Metrics**
Review pipeline execution logs for the prior month. Review items include:
- Collection failure rate per source
- Auto-retry invocation frequency
- Missing days or extended gaps in the Bronze Delta Lake tables
- Observation count trends per source vs. prior month

**Documentation and Findings**
Each Monthly Source Registry Review shall produce a brief written record (stored in the CCIR governance record) documenting:
- Review date and reviewer
- Any changes identified across the five review categories above
- Change classification: Non-Material Update (patch-level, no notice required), Non-Material Addition (minor, 14-day notice), or Material Change (major, 60-day notice)
- Pipeline or documentation changes made as a result of the review
- Any items deferred to the next review cycle

Findings classified as Non-Material Updates (e.g., model name string corrections, tier keyword additions) may be implemented immediately without advance notice. All other findings follow the change control process in Section 5.

---

## 10. Operational Resilience and Business Continuity

### 10.1 Infrastructure

The CCIR collection and calculation pipeline operates on Azure Databricks with ADLS Gen2 storage. Data is collected and processed through a Bronze/Silver/Gold Delta Lake architecture with automated daily backups.

### 10.2 Record Retention

CCIR shall retain the following records for a minimum of 7 years from the date of creation:

- All daily raw data snapshots with SHA-256 hashes
- All published Index values and associated metadata
- All change control documentation
- All Monthly Source Registry Review records
- All complaints and responses
- All governance decisions and Oversight Board minutes (once constituted)

All published data is retained in a public GitHub repository in perpetuity, subject to continued repository availability.

### 10.3 Business Continuity

In the event that CCIR is unable to continue operations as Administrator, CCIR shall:

- Provide maximum practicable advance notice to users
- Make all historical data available for download
- Use commercially reasonable efforts to identify and transition to a successor administrator
- Publish the Benchmark Replacement Waterfall guidance to facilitate user transition

### 10.4 Succession Planning

CCIR acknowledges that GPU-backed credit agreements may have multi-year terms and that Index cessation could create operational challenges for counterparties. CCIR commits to maintaining the publication series for a minimum of 24 months from the date of any material reduction in Index usage, and to providing maximum practicable notice of any planned cessation.

### 10.5 Credentials and Access Security

All API credentials, storage account keys, and pipeline access tokens used in Index collection shall be managed through a dedicated secrets management system (target: Azure Key Vault). Credentials shall not be stored in plaintext in pipeline code, notebooks, or version-controlled repositories. Credentials shall be rotated on a minimum annual basis. Any credential exposure shall be treated as a trigger-event for an immediate pipeline security review.

---

## 11. Audit and Independent Review

### 11.1 Internal Audit Trail

CCIR maintains an append-only, SHA-256-hashed audit trail linking every published Index value to its underlying raw data collection. This audit trail is publicly accessible via the GitHub repository, verifiable by any third party, and retained for a minimum of 7 years.

### 11.2 Pre-Transaction Audit

CCIR will provide, upon request, a pre-transaction audit attestation confirming the methodology version in effect on a specified date, that the published value was computed in accordance with that methodology, and the SHA-256 hash of the underlying raw data for the relevant calculation window. Requests: research@ccir.io. Response within 5 Business Days.

### 11.3 Annual Independent Audit

CCIR commits to commissioning an annual independent governance audit covering:

- Compliance of the collection and calculation pipeline with the published Methodology
- Adequacy of the conflict of interest controls
- Integrity of the audit trail and record retention
- Assessment of data source oversight practices, including Monthly Source Registry Review records

The first annual audit shall be commissioned within 18 months of first Index publication. Audit reports shall be published on the CCIR GitHub repository.

---

## 12. Complaints Procedure

### 12.1 Submission

Written complaints may be submitted to research@ccir.io with the subject line "FORMAL COMPLAINT."

### 12.2 Response Commitments

- **Acknowledgment:** Within 5 Business Days of receipt
- **Initial response:** Within 15 Business Days
- **Final response:** Within 30 Business Days of receipt

### 12.3 Escalation

If a complainant is not satisfied with CCIR's final response, the complaint may be escalated to the Oversight Board (once constituted).

### 12.4 Record

All complaints and CCIR's responses shall be retained in the governance record for 7 years. An anonymized summary shall be included in each Annual Benchmark Review.

---

## 13. Regulatory Cooperation

CCIR commits to cooperating with relevant regulatory authorities including responding to inquiries, providing access to records and data, and notifying relevant authorities of material issues affecting the Index. CCIR will engage proactively with any regulatory authority that asserts jurisdiction.

---

## 14. Intellectual Property and Licensing

### 14.1 Methodology and Governance Documents

The CCIR Methodology v1.0 and this Governance Framework v1.0 are published under Creative Commons Attribution 4.0 International (CC BY 4.0). Any party may freely cite, reproduce, or reference these documents with attribution to Compute Credit Index Research LLC.

### 14.2 Index Data

The published CRI-H100 and companion index data series are made available as open data. Any party may use, reproduce, and reference historical index values without charge. Attribution to CCIR is requested but not required for non-commercial use.

### 14.3 Commercial Licensing

Commercial use of CRI-H100 in financial products, structured products, or fee-bearing services requires a written license agreement. Inquiries: research@ccir.io.

### 14.4 Open-Source Pipeline Code

The pipeline code implementing the Methodology is published under the MIT License at https://github.com/ccir-index/cri-h100.

---

## 15. Rating Agency Disclosure Statement

### 15.1 Index Characteristics

| Characteristic | Description |
|----------------|-------------|
| Index type | Price index (listed rates, not executed transactions) |
| Data sources | Multiple public marketplaces: Vast.ai (primary), Shadeform, RunPod (secondary) |
| Market depth | Approximately 2,000–3,000 qualifying observations per day as of March 2026 (H100 SXM) |
| Calculation method | Trailing 7-day median-of-medians $/GPU-hour (one vote per source) |
| Outlier protection | IQR ± 3×, applied per source per collection day |
| Confidence flag | `high` (≥3 sources + ≥5 valid days), `medium` (≥2 sources + ≥3 valid days), `low` (otherwise) |
| Manipulation resistance | Median-of-medians; multi-source; append-only audit trail; public raw data |
| Reproducibility | Fully open-source; independently verifiable by any third party |
| Publication frequency | Daily |
| Governance | Rules-based; IOSCO-referenced; independent Oversight Board committed within 12 months |
| Monthly oversight | Source registry, nomenclature, and tier classification reviewed monthly (§9.4) |
| Audit | Annual independent governance audit committed; pre-transaction audit available on request |
| Complaints | Written procedure with 30-Business Day response commitment |

### 15.2 Material Risk Disclosures for Rating Analysis

**Listed vs. executed price basis.** The Index measures listed prices, not executed transaction prices. In a distressed collateral scenario, realized liquidation prices may differ materially from the CRI-H100 listed rate.

**Market immaturity.** The GPU rental market is evolving rapidly. The CRI-H100 historical series begins March 10, 2026. Rating agencies should apply conservative assumptions regarding rate stability and consider stress scenarios reflecting rapid price movement driven by new GPU generation releases.

**Tier mismatch.** CRI-H100 reflects Tier 3 secondary spot marketplace pricing. Borrowers with Tier 1 enterprise contracts will have a structural basis difference between their revenue rate and CRI-H100. This spread should be analyzed as part of collateral underwriting.

**Performance variance.** The Index does not adjust for intra-model GPU performance variation. See CCIR Methodology v1.0, Section 10.4.

**Source concentration.** Vast.ai is the primary data source. Disruption would trigger fallback provisions and Confidence Flag escalation. Secondary sources (Shadeform, RunPod) reduce but do not eliminate this concentration risk.

**Generational transition risk.** GPU compute markets undergo rapid generational transitions (H100 → H200 → B200). Rental yields for any given GPU generation may decline substantially over a 12–24 month horizon. CCIR's tier spread indices (H100/A100, H200/H100) are designed to track this dynamic. The Monthly Source Registry Review (§9.4) provides ongoing monitoring of migration patterns.

### 15.3 Haircut Guidance

CCIR does not provide haircut recommendations, as appropriate haircuts depend on transaction-specific factors. CCIR recommends that rating agencies and lenders conduct independent collateral valuation analysis accounting for the limitations disclosed in the CCIR Methodology and this Section 15.

---

## 16. Disclaimer

CRI-H100 and all companion indices are published for informational and reference purposes. The Indices do not constitute an offer, recommendation, or investment advice. No fiduciary relationship is created between CCIR and any market participant by reason of use of or reliance on any Index.

CCIR makes no representation that any Index reflects the price at which any particular GPU rental transaction could be executed, or the liquidation value of any GPU collateral portfolio.

CCIR uses commercially reasonable efforts to ensure accuracy and completeness but does not warrant that any Index is free from errors, omissions, or interruptions.

The Indices are provided "as is" without warranty of any kind, express or implied.

---

## Appendix A — Suggested Legal Citation Language

### A.1 Standard Reference Rate Definition

> **"Rental Rate Reference"** means, for any determination date, CRI-H100 as defined in CCIR Methodology v1.0, published by Compute Credit Index Research LLC at https://github.com/ccir-index/ccir-data/blob/main/cri-h100/CRI-H100-history.csv, representing the trailing 7-day median-of-medians $/GPU-hour for H100 SXM on-demand rental listings on the US spot marketplace, published daily pursuant to the CCIR Governance Framework v1.0.

### A.2 Methodology Version Pinning

> The Rental Rate Reference shall be calculated in accordance with the CCIR Methodology as of the git commit hash [●], available at https://github.com/ccir-index/ccir-methodology/commit/[●]. Any Material Methodology Change (as defined in CCIR Governance Framework v1.0, Section 5.2) shall be subject to the notification and renegotiation provisions of Section [●] of this Agreement.

### A.3 Fallback Language

> In the event that the Rental Rate Reference is not published on any scheduled publication date, the Rental Rate Reference for such date shall be determined pursuant to the fallback waterfall set forth in Section 7.1 of the CCIR Governance Framework v1.0. In the event that no CRI-H100 value has been published within the prior thirty (30) calendar days, the Rental Rate Reference shall be determined pursuant to the benchmark replacement waterfall in Section 7.2 of the CCIR Governance Framework v1.0.

### A.4 Confidence Flag Handling

> In the event that the Rental Rate Reference for any determination date is published with a `low` Confidence Flag (as defined in CCIR Methodology v1.0, Section 7.1), the Rental Rate Reference for such date shall be the most recently published CRI-H100 value bearing a `medium` or `high` Confidence Flag.

### A.5 Material Change Notification

> The Borrower shall notify the Administrative Agent within five (5) Business Days of receiving notice from CCIR of any Material Methodology Change (as defined in CCIR Governance Framework v1.0, Section 5.2) affecting CRI-H100. If a Material Methodology Change results in a change to the Rental Rate Reference of more than [●]% relative to the prior methodology, the parties shall negotiate in good faith to agree an alternative Rental Rate Reference within thirty (30) days of the effective date of such change.

---

## Appendix B — Version History

| Version | Effective Date | Change Type | Summary |
|---------|---------------|-------------|---------|
| v1.0 | Mar 10, 2026 | Initial public release | Establishes Administrator/Calculation Agent framework, governance structure, change control, fallback waterfall, monthly source registry review (§9.4), credentials security (§10.5), complaints procedure, regulatory cooperation, IP and licensing, rating agency disclosure. Designed with reference to IOSCO Principles for Financial Benchmarks (2013). |

---

## Appendix C — IOSCO Principles Alignment Summary

| # | Principle | Primary Reference |
|---|-----------|-------------------|
| 1 | Overall Responsibility of the Administrator | §§1–2 |
| 2 | Oversight of Third Parties | §9.3 |
| 3 | Conflicts of Interest for Administrators | §3 |
| 4 | Control Framework | §§2, 8, 9.2 |
| 5 | Internal Oversight Function | §§4.2, 4.3, 11.3 |
| 6 | Benchmark Design | Methodology §§5–6 |
| 7 | Data Sufficiency | §4.3; Methodology §7.1 |
| 8 | Hierarchy of Data Inputs | §2.4 |
| 9 | Transparency of Benchmark Determinations | §§6, 8, 9.2 |
| 10 | Periodic Review | §§4.3, 9.4 |
| 11 | Content of the Methodology | Methodology v1.0 |
| 12 | Changes to the Methodology | §5 |
| 13 | Transition | §§7, 10.3, 10.4 |
| 14 | Submitter Code of Conduct | Not applicable — no submissions accepted (§2.4) |
| 15 | Internal Controls over Data Collection | §9.2 |
| 16 | Complaints Procedures | §12 |
| 17 | Audits | §11 |
| 18 | Audit Trail | §§10.2, 11.1 |
| 19 | Cooperation with Regulatory Authorities | §13 |

---

*This document is published under Creative Commons Attribution 4.0 International (CC BY 4.0).*
*For the technical index calculation specification, see the companion CCIR Methodology v1.0.*
*© 2026 Compute Credit Index Research LLC*
