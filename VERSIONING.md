# CCIR Methodology Versioning Policy

---

## Overview

CCIR uses semantic versioning for all methodology documents. The version number takes the form **vMAJOR.MINOR.PATCH** (e.g., v1.1.0).

This policy governs:
- What constitutes a material vs. non-material change
- Notice requirements before implementation
- How existing credit agreements are affected
- How to reference a specific methodology version in legal documents

---

## Version Component Definitions

### MAJOR (e.g., v1 → v2)
A **major version increment** indicates a material change to the methodology — one that could produce a different published index value for an identical underlying dataset, or that materially alters the scope, coverage, or governance of the index.

**Examples of major changes:**
- Changing the central tendency measure (e.g., median → trimmed mean)
- Changing the outlier removal threshold (e.g., 2.5σ → 2.0σ)
- Changing the calculation window (e.g., 7 days → 14 days)
- Adding a new data source that materially affects the index level
- Changing the geographic scope of the primary index
- Changing the quality filter thresholds in a way that materially alters the qualifying observation set

**Notice and process:**
- 60-day advance notice to all parties with registered contracts referencing CRI-H100
- 30-day stakeholder comment period
- Formal Oversight review and approval
- Existing contracts continue to reference the prior major version unless explicitly amended
- Both the prior and new methodology versions remain permanently available in this repository

### MINOR (e.g., v1.0 → v1.1)
A **minor version increment** indicates a non-material improvement — one that clarifies, extends, or improves the methodology without changing the published index value for identical underlying data.

**Examples of minor changes:**
- Adding formal definitions that clarify existing practice
- Adding pseudocode or additional technical documentation
- Adding a new monitored metric that does not affect the primary calculation
- Adding a new supplementary index (e.g., CRI-A100) under the same methodology
- Clarifying ambiguities in filter application without changing the filter criteria

**Notice and process:**
- 14-day advance notice
- No formal comment period required (though stakeholder input is welcomed)
- Contracts referencing "v1.x" automatically follow minor version updates
- Contracts referencing a specific minor version (e.g., "v1.0") are not affected

### PATCH (e.g., v1.1.0 → v1.1.1)
A **patch increment** indicates a typographical correction, formatting improvement, or cross-reference fix only. No substantive change to calculation, process, or governance.

**Notice and process:**
- No advance notice required
- CHANGELOG.md updated at time of publication
- No contract implications

---

## Version Table

| Version | Date | Type | Material? | Summary |
|---------|------|------|-----------|---------|
| v1.1.0 | Feb 2026 | Initial publication | No | CRI-H100 initial methodology, multi-model collection |
| v1.2.0 | TBD | Minor | No | Planned: CRI-A100 formal publication |
| v2.0.0 | TBD | Major | Yes | Planned: CRI-S multi-source framework |

---

## How to Reference a Methodology Version in a Legal Document

### For maximum flexibility (follows non-material updates automatically):
> *"...calculated in accordance with CCIR Methodology v1.x (as amended from time to time in accordance with the non-material change provisions of CCIR Governance Framework v1.0)..."*

### For a fixed methodology (no amendments):
> *"...calculated in accordance with CCIR Methodology v1.1.0 as published at commit [SHA] of https://github.com/ccir-index/ccir-methodology..."*

### For transactions requiring notification of any change:
> *"...calculated in accordance with CCIR Methodology v1.1.0, provided that any change to the methodology (whether material or non-material) shall require 30 days' prior written notice to Lender..."*

---

## Permanent Availability

All prior versions of CCIR methodology documents remain permanently available in this repository via Git history. Any published version can be accessed by referencing its commit SHA, ensuring that any party can reconstruct the exact methodology in effect at any point in time.

---

© 2026 Compute Credit Index Research LLC · CC BY 4.0
