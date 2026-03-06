# CCIR Methodology Repository

**Compute Credit Index Research (CCIR)**
[ccir.io](https://ccir.io) · [research@ccir.io](mailto:research@ccir.io) · [Data Repository](https://github.com/ccir-index/cri-h100)

---

## What This Repository Is

This repository is the authoritative methodology and governance record for the CRI family of GPU rental rate indices published by Compute Credit Index Research (CCIR). It contains the complete technical specification, governance framework, and supporting documentation required to understand, verify, and cite CRI indices in credit agreements, loan covenants, and collateral valuation frameworks.

All documents in this repository are versioned, change-controlled, and subject to the formal governance process described in `GOVERNANCE-v1_0.md`.

---

## Scope and Definitions

**What this repository covers.** This repository governs the CRI family of GPU rental rate indices — open-methodology benchmarks designed for credit document citation. The live indices are CRI-H200 (NVIDIA H200, next-generation training tier), CRI-H100 (NVIDIA H100 SXM, training tier), and CRI-L40S (NVIDIA L40S, inference tier): each a weekly median $/GPU-hour for on-demand listings on the US Vast.ai marketplace.

**What this repository does not cover.** This repository does not contain the underlying daily data (archived at [github.com/ccir-index/cri-h100](https://github.com/ccir-index/cri-h100)), the verification pipeline code (also in cri-h100), or CCIR's advisory and administrator service agreements.

**Key terms.** Throughout this repository, *methodology* refers to the calculation specification (how the index value is computed from raw data); *governance* refers to the administrative framework (how the index is overseen, changed, and audited); *index* refers to the published CRI-H100 value. These are distinct: a party may verify the methodology independently without any governance relationship with CCIR.

---

## Repository Structure

```
ccir-methodology/
├── README.md                  ← This file — start here
├── METHODOLOGY-v1.1.1.md      ← Full technical specification for CRI-H100
├── GOVERNANCE-v1_0.md         ← Administrator framework, IOSCO alignment, change control
├── CHANGELOG.md               ← Version history and change log
├── VERSIONING.md              ← Semantic versioning policy
├── REPRODUCIBILITY.md         ← Step-by-step independent verification guide
└── FUTURE-EXTENSIONS.md       ← Planned index extensions and roadmap
```

---

## How the Documents Relate

| Document | Purpose | Audience |
|----------|---------|----------|
| `METHODOLOGY-v1.1.1.md` | Defines exactly how CRI-H100 is calculated | Analysts, attorneys, rating agencies, third-party verifiers |
| `GOVERNANCE-v1_0.md` | Defines how CCIR is administered, change-controlled, and audited | Lenders, structurers, rating agencies, regulators |
| `REPRODUCIBILITY.md` | Step-by-step guide to independently verifying any published value | Any third party seeking to audit a specific index value |
| `CHANGELOG.md` | Complete version history with change descriptions | Anyone tracking methodology evolution |
| `VERSIONING.md` | Defines what constitutes a material vs. non-material change | Parties with contractual references to methodology versions |
| `FUTURE-EXTENSIONS.md` | Planned index extensions (CRI-S, CRI-D, regional variants) | Lenders and structurers planning future covenant structures |

---

## Benchmark Integrity and Independence

CRI-H100 is designed around structural independence — a property that cannot be achieved through policy alone when a benchmark administrator has commercial exposure to the market they measure.

**CCIR does not operate a GPU rental marketplace.** CCIR has no commercial interest in GPU rental prices, transaction volume, or the financial performance of any marketplace operator. This means CCIR has no financial incentive to influence the index in any direction.

**CCIR does not accept submissions.** CRI-H100 is calculated from publicly available API data. There are no panel banks, no contributor banks, and no submitter relationships. The LIBOR-style conflicts that prompted the IOSCO Principles for Financial Benchmarks do not apply.

**CCIR does not own GPU hardware or hold GPU-backed loans.** CCIR has no inventory exposure to GPU collateral value and is not a GPU lender, lessor, or hardware broker.

**The methodology is independently reproducible without CCIR's cooperation.** Any party can query the same Vast.ai API endpoint, apply the published filters, and reproduce any CRI-H100 value. No data license, terminal subscription, or relationship with CCIR is required. See `REPRODUCIBILITY.md`.

These are structural facts, not policy commitments. See `GOVERNANCE-v1_0.md` §§3.1 and 3.5 for the full elaborated independence disclosure, including a due diligence checklist for evaluating competing benchmark administrators.

---

## How to Use This Repository

**If you are a lender or structurer** evaluating CRI-H100 for a credit agreement:
1. Read `METHODOLOGY-v1.1.1.md` §3 ("What CRI-H100 Measures — and Does Not Measure") to confirm the index is appropriate for your use case
2. Review the covenant templates in the CCIR white paper (linked at ccir.io)
3. Use the citation language in `GOVERNANCE-v1_0.md` Appendix A for your legal documents
4. Note the commit hash pinning guidance below if your transaction requires a fixed methodology version

**If you are a rating agency analyst** evaluating CRI-H100 for a structured product:
1. Start with `GOVERNANCE-v1_0.md` §15 (Rating Agency Disclosure Statement)
2. Review the IOSCO alignment summary in Governance Appendix C
3. Review `METHODOLOGY-v1.1.1.md` §10 (Known Limitations) for the material risk disclosures
4. Use `REPRODUCIBILITY.md` to independently verify the calculation against raw data

**If you are an attorney** drafting CRI-H100 into a credit document:
1. Use the citation language in `GOVERNANCE-v1_0.md` Appendix A
2. Review the commit hash pinning guidance below for fixed-version transactions
3. Review `VERSIONING.md` for how major vs. minor changes affect contractual references
4. Review Governance §§5 and 7 for change control and fallback provisions

**If you are auditing or verifying a published index value:**
1. Follow `REPRODUCIBILITY.md` step by step
2. Use `pipeline/verify.py` in the [cri-h100 repository](https://github.com/ccir-index/cri-h100)
3. Cross-reference the published audit file at `outputs/audits/cri-h100-YYYY-MM-DD.audit.json`

---

## How to Verify an Index Value

Any published CRI-H100 value can be independently reproduced by any third party. The full process is documented in `REPRODUCIBILITY.md`. In brief:

```bash
git clone https://github.com/ccir-index/cri-h100
cd cri-h100
pip install -r requirements.txt
python pipeline/verify.py --end-date YYYY-MM-DD
```

The verification script compares your independently computed value against the published value using the raw data files and SHA-256 hashes committed at the time of collection.

---

## How to Cite the Methodology

When referencing CRI-H100 in a credit agreement or legal document, cite the methodology version in effect at the time of the agreement:

> *"The Rental Rate Reference shall be the CRI-H100 index published by Compute Credit Index Research LLC, calculated in accordance with CCIR Methodology v1.1.1 (https://github.com/ccir-index/ccir-methodology/blob/main/METHODOLOGY-v1.1.1.md), as may be amended pursuant to the Change Control provisions of CCIR Governance Framework v1.0."*

For transactions requiring a fixed methodology version (no amendments), reference the specific commit hash rather than the file path.

### Commit Hash Pinning

Every version of every document in this repository is permanently available via its Git commit SHA. To pin a transaction to an exact methodology snapshot:

1. Navigate to the file on GitHub (e.g., `METHODOLOGY-v1.1.1.md`)
2. Click **History** → select the relevant commit → copy the full 40-character SHA
3. Reference it in your legal document:

> *"...calculated in accordance with CCIR Methodology v1.1.1 as published at commit `[40-char SHA]` of https://github.com/ccir-index/ccir-methodology, which methodology shall not be subject to amendment for purposes of this Agreement."*

This provides an immutable, court-verifiable reference to the exact methodology in effect at signing — independent of any future CCIR document updates.

---

## Data Source Independence

CRI-H100 is calculated from the Vast.ai public API — an unauthenticated endpoint accessible to any party without a data license, terminal subscription, or agreement with CCIR or Vast.ai.

**Endpoint:** `https://console.vast.ai/api/v0/bundles/`

Any attorney, analyst, rating agency, or counterparty can independently query this endpoint, apply the published filters, and reproduce CCIR's inputs. This is not a courtesy — it is a design requirement. A reference rate that cannot be independently reproduced from public sources does not meet the governance standard for credit document citation in a post-LIBOR regulatory environment.

Contrast with proprietary benchmark feeds distributed via terminal subscriptions: a party seeking to audit a covenant calculation cannot reproduce the underlying data without the benchmark administrator's cooperation. CRI-H100 has no such dependency.

---

## How Versioning Works

CCIR uses semantic versioning for methodology documents:

- **Major version** (e.g., v1.0 → v2.0): Material change. 60-day notice, 30-day stakeholder consultation, formal Oversight review. Existing contracts reference the prior version unless explicitly amended.
- **Minor version** (e.g., v1.0 → v1.1.1): Non-material improvement. 14-day notice. Contracts referencing "v1.x" automatically follow minor updates.
- **Patch** (e.g., v1.1.1 → v1.1.1): Clarification or typographical correction only. No substantive change to calculation.

See `VERSIONING.md` for the full policy and `CHANGELOG.md` for the complete version history.

---

## How Change Control Works

Methodology changes follow the process defined in `GOVERNANCE-v1_0.md` Section 5:

1. Proposed change documented and published for stakeholder comment
2. Comment period (30 days for material changes, 14 days for non-material)
3. Oversight review and approval
4. Implementation with appropriate notice
5. CHANGELOG.md updated with full description

Emergency changes (data source failure, manifest error) may be implemented immediately with retrospective documentation. See Governance §5.3.

---

## How to Interpret the Low Confidence Flag

CRI-H100 publishes a Low Confidence flag when:
- Daily qualifying observations fall below 10, or
- Fewer than 3 valid collection days exist in the 7-day calculation window

A Low Confidence publication is not suppressed — it is published with explicit disclosure. A directional estimate with disclosed uncertainty is more useful for credit markets than a data gap. Lenders should calibrate covenant trigger levels to account for confidence regime. See Methodology §5.7 and §7.5 for the full confidence analysis framework.

---

## How to File a Complaint

Complaints regarding index calculation, methodology interpretation, or governance process should be directed to [research@ccir.io](mailto:research@ccir.io). CCIR commits to acknowledging all complaints within 5 Business Days and responding within 30 Business Days. The full complaints procedure is in `GOVERNANCE-v1_0.md` Section 12.

---

## Current Index Family

| Index | Tier | Status | Description |
|-------|------|--------|-------------|
| CRI-H100 | Training | **Live** — first publication March 2026 | Weekly median H100 SXM rental rate, US marketplace |
| CRI-H200 | Training (next-gen) | **Live** — first publication March 2026 | Weekly median H200 rental rate, US marketplace. Next-generation training-tier reference rate |
| CRI-L40S | Inference | **Live** — first publication March 2026 | Weekly median L40S rental rate, US marketplace. Inference-tier reference rate |
| CRI-S | Cross-tier spread | Planned — Q3-Q4 2026 | Rental-resale spread index (requires broker data partnership) |
| CRI-D | Depreciation curves | Planned — Q1-Q2 2027 | Cross-generational depreciation curves (requires 12-18 months CRI data) |
| CRI-H100-PA | Performance-adjusted | Roadmap | Performance-adjusted variant (requires reproducible benchmark methodology) |
| CRI-H100-EU | Regional | Roadmap | European geography, identical methodology |
| CRI-H100-APAC | Regional | Roadmap | Asia-Pacific geography, identical methodology |

See `FUTURE-EXTENSIONS.md` for full roadmap detail.

---

## License

Methodology documents: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to cite, use, and reference with attribution.
Verification code: [MIT](https://github.com/ccir-index/cri-h100/blob/main/LICENSE)

---

© 2026 Compute Credit Index Research LLC
