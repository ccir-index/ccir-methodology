# CCIR Versioning Policy

**Applies to:** CCIR Methodology and CCIR Governance Framework
**Current methodology version:** v1.0.0 (March 2026)
**Current governance version:** v1.0.0 (March 2026)

This document defines what constitutes a material vs. non-material change, the notice periods required for each, and how existing contracts are affected when the methodology is updated.

---

## Semantic Versioning

CCIR uses semantic versioning (`MAJOR.MINOR.PATCH`) for the Methodology. The Governance Framework uses `MAJOR.MINOR`.

| Version component | Classification | Notice required | Example |
|-------------------|---------------|----------------|---------|
| **MAJOR** (`N.x.x`) | Material Methodology Change | 60 days + 30-day stakeholder consultation | Change to calculation statistic, data source removal |
| **MINOR** (`x.N.x`) | Non-material addition or expansion | 14 days | New companion index under identical methodology |
| **PATCH** (`x.x.N`) | Non-material clarification | None | Typographic correction, cross-reference fix |

Governance Framework changes follow the same materiality classification. Governance version increments accompany methodology version increments when governance provisions change.

---

## What Constitutes a Material Methodology Change

The following changes are **Material** and require 60-day advance notice plus 30-day stakeholder consultation:

- Change to the primary calculation statistic (e.g., median → trimmed mean)
- Change to the calculation window length (e.g., 7 days → 14 days)
- Removal of a contributing data source
- Change to the outlier removal procedure (fence multiplier, grouping key, or method)
- Change to the geographic scope of any index
- Change to the hardware filter definition for any index
- Introduction of expert judgment or discretionary inputs to routine calculation
- Cessation or suspension of any index

The following changes are **not** material:

- Addition of a new data source alongside existing sources
- Addition of a new companion index under the identical methodology
- Correction of a Manifest Error via Restatement (which is not a methodology change)
- Increase in collection or publication frequency without changing the calculation window or statistic
- Addition of a new GPU model to the collection pipeline
- Documentation clarifications, additional disclosures, or cross-reference corrections
- Source-level field mapping updates

---

## Notice Periods and Process

### Material Changes (60-day notice)

1. CCIR publishes the proposed change with a 30-day written comment period. Comments to research@ccir.io.
2. CCIR publishes a summary of comments received and CCIR's response.
3. A 60-day advance notice period begins from the date of proposed change publication.
4. The new methodology takes effect at the end of the 60-day notice period.
5. A new METHODOLOGY file is added to this repository. The prior version file is retained unchanged.

### Non-material Changes (14-day notice)

1. CCIR publishes a notice of the proposed change.
2. The change takes effect 14 days after publication of the notice.
3. A new METHODOLOGY file is added if the change affects the calculation specification.

### Patch Changes (no notice)

Applied immediately. No new METHODOLOGY file unless calculation-relevant.

---

## Contract Pinning

Credit agreements, loan documents, and legal instruments referencing CCIR indices should pin the specific methodology version they rely on. CCIR commits to preserving all historical methodology files in this repository indefinitely, even after supersession.

**How to pin:**

1. Reference the specific file by name: `CCIR Methodology v1.0.0` (`METHODOLOGY-v1.0.0.md`)
2. For maximum precision, pin by git commit SHA:
   ```
   CCIR Methodology as of commit [SHA], available at
   https://github.com/ccir-index/ccir-methodology/commit/[SHA]
   ```
3. Include Material Methodology Change notification language. Template: Governance Framework Appendix A.2.

**Effect of a new methodology version on pinned contracts:**

A contract pinned to a specific version continues to reference the calculation rules in effect under that version. Published index values include a `methodology_version` column in the CSV; contracts should verify this field when using historical values. CCIR recommends that contracts include the Material Methodology Change notification language in Governance Framework Appendix A.5.

---

## Index Family Version Applicability

All benchmark indices in the CRI family share a single methodology version. A version bump applies to all indices simultaneously.

| Index | Governed by |
|-------|-----------|
| CRI-H100 | CCIR Methodology v1.0.0 |
| CRI-A100 | CCIR Methodology v1.0.0 |
| CRI-L40S | CCIR Methodology v1.0.0 |
| CRI-H200 | CCIR Methodology v1.0.0 |
| CRI-B200 | CCIR Methodology v1.0.0 |
| CRI-R | CCIR Methodology v1.0.0 (reference series only — not a benchmark) |
| CRI-S | Separate methodology document (planned) |

CRI-S will have its own methodology document and version chain when published, reflecting its distinct data inputs (brokerage resale transaction data) and calculation requirements.

---

*For governance provisions governing change control, consultation, and stakeholder notification, see [`GOVERNANCE-v1.0.0.md`](./GOVERNANCE-v1.0.0.md) §5.*

*© 2026 Compute Credit Index Research LLC. Published under CC BY 4.0.*
