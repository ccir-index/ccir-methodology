# CCIR Methodology and Governance

**Compute Credit Index Research** · [ccir.io](https://ccir.io) · [research@ccir.io](mailto:research@ccir.io)

This repository contains the published methodology and governance framework for the CRI family of GPU compute reference rates. It is the authoritative documentation source for structured finance counsel, rating agency staff, and credit analysts evaluating whether CRI rates may be cited in credit agreements, collateral appraisals, and covenant frameworks.

---

## Documents

| Document | Version | Description |
|---|---|---|
| [CCIR_Methodology_v1_0_0.pdf](./CCIR_Methodology_v1_0_0.pdf) | v1.0.0 · March 2026 | Index construction methodology — eligibility criteria, computation procedures, confidence flag definitions, source taxonomy, and spread framework |
| [CCIR_Governance_v1_0_0.pdf](./CCIR_Governance_v1_0_0.pdf) | v1.0.0 · March 2026 | Governance and IOSCO Alignment Statement — detailed crosswalk of all 19 IOSCO Principles for Financial Benchmarks |
| [CCIR_Benchmark_Design_Rationale_v1_0_0.pdf](./CCIR_Benchmark_Design_Rationale_v1_0_0.pdf) | v1.0.0 · March 2026 | Benchmark design rationale — construction choices, technical rationale, and underwriter implications for each index design decision |
| [CCIR_Change_Management_Policy_v1_0_0.pdf](./CCIR_Change_Management_Policy_v1_0_0.pdf) | v1.0.0 · March 2026 | Benchmark change management and versioning policy — change classification, notice periods, consultation requirements, and historical data preservation |
| [CCIR_Data_Source_Quality_Policy_v1_0_0.pdf](./CCIR_Data_Source_Quality_Policy_v1_0_0.pdf) | v1.0.0 · March 2026 | Data source quality policy — source eligibility criteria, shadow period and integration process, ongoing monitoring cadence, source coverage standards, and publication hold thresholds |
| [CCIR_Fallback_Language_Guidance_v1_0_0.pdf](./CCIR_Fallback_Language_Guidance_v1_0_0.pdf) | v1.0.0 · March 2026 | Fallback language guidance for credit agreements — benchmark definition language, trigger events, fallback rate waterfall, and successor benchmark mechanism |
| [CCIR_Complaints_Procedure_v1_0.pdf](./CCIR_Complaints_Procedure_v1_0.pdf) | v1.0 · March 2026 | Complaints procedure — scope, process, determination timelines, corrective action standards, and Oversight Board escalation path |

---

## What CCIR Publishes

The CRI index family covers five GPU models across spot and cluster tiers:

**Spot indices** — on-demand, noninterruptible, per-GPU-per-hour  
`CRI-B200` · `CRI-H200` · `CRI-H100` · `CRI-A100` · `CRI-L40S`

**Cluster indices — mid-market (MM)** — marketplace spot cluster rates, no term commitment  
`CRI-H100-CLS-MM` · `CRI-A100-CLS-MM` · `CRI-B200-CLS-MM`

**Cluster indices — enterprise (ENT)** — posted rate card, bundled infrastructure  
`CRI-H100-CLS-ENT` · `CRI-H200-CLS-ENT` · `CRI-B200-CLS-ENT`

**Generation Spread** — CRI-B200 vs CRI-H100, and CRI-H100 vs CRI-A100  
The primary collateral obsolescence signal for §7.04 Generation Spread covenants.

Published rate data is available at [ccir-index/ccir-data](https://github.com/ccir-index/ccir-data).

---

## Structural Independence

CCIR holds no financial interest — equity, revenue share, referral fee, or data licensing income — in any GPU compute marketplace, cloud provider, or data center operator from which it collects data. All data is collected algorithmically from public APIs and published rate cards. No source submits pricing to CCIR; CCIR retrieves publicly posted prices. CCIR does not participate in or structure credit transactions referencing CRI rates, and does not provide advice regarding any such transaction.

This structural independence is the organizational precondition for CRI indices to function as citation-grade reference instruments in credit documents. In a post-LIBOR environment, a benchmark cited in a credit agreement must be produced by an administrator that is independent from the market it measures. Lender-controlled metrics and marketplace-produced rates are both disqualified on this basis. CCIR's independence from all GPU marketplaces resolves both conditions.

---

## IOSCO Alignment

CCIR maintains voluntary alignment with the [IOSCO Principles for Financial Benchmarks (July 2013)](https://www.iosco.org/library/pubdocs/pdf/IOSCOPD415.pdf) — the structural standard structured finance counsel and rating agencies use to evaluate benchmark fitness for credit document citation.

Of the 19 IOSCO Principles:
- **17 — Compliant** as of March 2026
- **2 — Targeted** (Oversight Board formation Q1 2027; annual independent audit first full calendar year)
- **3 — Not Applicable** (P11, P12, P14 — submitter code of conduct, submitter code of conduct content, and submitter panels; CCIR has no human submitters)

Full crosswalk in [CCIR_Governance_v1_0_0.pdf](./CCIR_Governance_v1_0_0.pdf).

---

## Governance Milestones

| Item | Status | Target |
|---|---|---|
| Dual automated audit framework | Operational | — |
| Methodology v1.0.0 publication | Complete | March 2026 |
| Complaints procedure publication | Complete | March 2026 |
| Data Source Quality Policy publication | Complete | March 2026 |
| Monthly model string review | Operational | — |
| Annual independent audit | Targeted | First full calendar year |
| Oversight Board formation | Targeted | Q1 2027 |

---

## Versioning

Every published index value carries a methodology version stamp. Historical values computed under a prior methodology version are never restated. This non-restatement principle is intended to preserve contractual certainty for financial agreements referencing CRI indices. Any filter change, computation change, or source inclusion change requires documentation of the rationale and a version bump with prospective application only.

---

## Contact

Methodology questions, source inclusion inquiries, and formal complaints: [research@ccir.io](mailto:research@ccir.io)  
Written response within 10 business days.
