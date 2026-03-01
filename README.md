# CCIR Methodology Repository

**Compute Credit Index Research (CCIR)**
[ccir.io](https://ccir.io) · [research@ccir.io](mailto:research@ccir.io) · [Data Repository](https://github.com/ccir-index/cri-h100)

---

## What This Repository Is

This repository is the authoritative methodology and governance record for the CRI family of GPU rental rate indices published by Compute Credit Index Research (CCIR). It contains the complete technical specification, governance framework, and supporting documentation required to understand, verify, and cite CRI indices in credit agreements, loan covenants, and collateral valuation frameworks.

All documents in this repository are versioned, change-controlled, and subject to the formal governance process described in `GOVERNANCE-v1_0.md`.

---

## Repository Structure

```
ccir-methodology/
├── README.md                  ← This file — start here
├── METHODOLOGY-v1.1.0.md      ← Full technical specification for CRI-H100
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
| `METHODOLOGY-v1.1.0.md` | Defines exactly how CRI-H100 is calculated | Analysts, attorneys, rating agencies, third-party verifiers |
| `GOVERNANCE-v1_0.md` | Defines how CCIR is administered, change-controlled, and audited | Lenders, structurers, rating agencies, regulators |
| `REPRODUCIBILITY.md` | Step-by-step guide to independently verifying any published value | Any third party seeking to audit a specific index value |
| `CHANGELOG.md` | Complete version history with change descriptions | Anyone tracking methodology evolution |
| `VERSIONING.md` | Defines what constitutes a material vs. non-material change | Parties with contractual references to methodology versions |
| `FUTURE-EXTENSIONS.md` | Planned index extensions (CRI-S, CRI-D, regional variants) | Lenders and structurers planning future covenant structures |

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

> *"The Rental Rate Reference shall be the CRI-H100 index published by Compute Credit Index Research LLC, calculated in accordance with CCIR Methodology v1.1.0 (https://github.com/ccir-index/ccir-methodology/blob/main/METHODOLOGY-v1.1.0.md), as may be amended pursuant to the Change Control provisions of CCIR Governance Framework v1.0."*

For transactions requiring a fixed methodology version (no amendments), reference the specific commit hash rather than the file path.

---

## How Versioning Works

CCIR uses semantic versioning for methodology documents:

- **Major version** (e.g., v1.0 → v2.0): Material change. 60-day notice, 30-day stakeholder consultation, formal Oversight review. Existing contracts reference the prior version unless explicitly amended.
- **Minor version** (e.g., v1.0 → v1.1): Non-material improvement. 14-day notice. Contracts referencing "v1.x" automatically follow minor updates.
- **Patch** (e.g., v1.1.0 → v1.1.1): Clarification or typographical correction only. No substantive change to calculation.

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

| Index | Status | Description |
|-------|--------|-------------|
| CRI-H100 | **Live** — first publication March 2026 | Weekly median H100 SXM rental rate, US marketplace |
| CRI-S | Planned — Q3-Q4 2026 | Rental-resale spread index (requires broker data partnership) |
| CRI-D | Planned — Q1-Q2 2027 | Cross-generational depreciation curves (requires 12-18 months CRI-H100 data) |
| CRI-H100-PA | Roadmap | Performance-adjusted variant (requires reproducible benchmark methodology) |
| CRI-H100-EU | Roadmap | European geography, identical methodology |
| CRI-H100-APAC | Roadmap | Asia-Pacific geography, identical methodology |

See `FUTURE-EXTENSIONS.md` for full roadmap detail.

---

## License

Methodology documents: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to cite, use, and reference with attribution.
Verification code: [MIT](https://github.com/ccir-index/cri-h100/blob/main/LICENSE)

---

© 2026 Compute Credit Index Research LLC
