# CCIR Future Extensions

This document describes planned extensions to the CRI index family. All extensions will be published under the same open-methodology governance framework as CRI-H100.

Extensions are listed in expected publication sequence. Timelines are estimates and are contingent on data availability, governance review, and market conditions.

---

## Stage 1.5 — CRI-S: Rental-Resale Spread Index

**Target:** Q3–Q4 2026 (contingent on broker data partnership)
**Methodology version:** Separate document — METHODOLOGY-CRI-S-v1.0.md

### What It Measures
CRI-S measures the spread between rental rate depreciation (δ_r) and hardware resale depreciation (δ_v) for each GPU generation:

```
S(g, t) = δ_r(g, t) - δ_v(g, t)
```

When S > 0, rental rates have declined faster than resale values — favorable for lenders (the liquidation floor is holding).
When S < 0, resale values have declined faster — the liquidation floor is deteriorating faster than the revenue signal suggests.

### Data Requirements
- **Rental rate input:** CRI-H100 (available now)
- **Resale rate input:** Aggregated broker-contributed indicative bid prices for standard GPU configurations in standard lot sizes (10-unit, 50-unit, 100-unit)

### Aggregation Model
Broker contributions will be aggregated using a SOFR-style median: individual broker submissions are confidential; only the aggregate median is published. This is structurally analogous to established commodity price assessments (Platts, OPIS).

### Minimum Viable Launch
One broker contributing weekly H100 SXM indicative bids is sufficient to launch a single-contributor CRI-S with appropriate disclosure. Three or more contributors enables meaningful aggregation.

### Credit Application
CRI-S is the metric credit analysts need for real-time collateral coverage monitoring — it directly measures whether the liquidation floor is holding or deteriorating. A sustained negative spread is an early warning of cascade failure.

---

## Stage 2 — CRI-D: Cross-Generational Depreciation Curves

**Target:** Q1–Q2 2027 (requires 12–18 months of CRI-H100 data)
**Methodology version:** Separate document — METHODOLOGY-CRI-D-v1.0.md

### What It Measures
CRI-D derives empirical depreciation curves from cross-generational CRI time series. It measures observed rental rate decline by GPU generation, providing:
- Historical depreciation curves (V100, A100, H100)
- Forward depreciation scenarios (Gradual/Accelerated/Rapid)
- Cross-generational transition timing

### Data Requirements
- Minimum 52 weekly CRI-H100 observations (one full year)
- CRI-A100 and CRI-V100 indices operating under identical methodology
- At least one observable technology transition in the data series

### Credit Application
CRI-D replaces management-estimate depreciation assumptions with market-observed curves. It is the primary input for:
- LGD modeling in GPU-backed loans
- ABS pool depreciation stress testing
- Rating agency sensitivity analysis

---

## CRI-H100-PA: Performance-Adjusted Variant

**Target:** TBD (contingent on reproducible benchmark methodology)
**Methodology version:** Separate document — METHODOLOGY-CRI-H100-PA-v1.0.md

### What It Measures
CRI-H100 measures listed rental rates without adjusting for intra-model performance variance. Research published at GPGPU '26 (Silicon Data, March 2026) documents up to 38% performance variation within the H100 SXM model.

CRI-H100-PA will adjust the raw price signal for measured performance differences, producing a compute-per-dollar index rather than a price-per-GPU-hour index.

### Data Requirements
A standardized, reproducible performance grading methodology. Options under evaluation:
- Independent benchmark suite run by CCIR on sampled providers
- Licensed performance data from a third party (subject to open-methodology compatibility)
- Community benchmark dataset with sufficient coverage and reproducibility

### Relationship to CRI-H100
CRI-H100-PA complements rather than replaces CRI-H100. The two indices answer different questions:
- CRI-H100: What does the market charge per GPU-hour?
- CRI-H100-PA: What does the market charge per unit of standardized compute?

Both are useful for credit analysis. CRI-H100 is appropriate for price-based covenant triggers. CRI-H100-PA is appropriate for collateral valuation adjusted for actual compute capacity.

---

## Regional Variants

**Target:** TBD
**Methodology version:** Same as primary index — geographic filter only change

### CRI-H100-EU
European Union listings under identical methodology. Captures geographic price structure and enables analysis of US-EU rental rate spreads.

### CRI-H100-APAC
Asia-Pacific listings under identical methodology.

### Cross-Regional Spread
Publication of regional variants enables formal analysis of geographic price structure — the compute analogue to locational marginal pricing in electricity markets. Preliminary evidence suggests persistent regional price divergence that is informative for lenders with geographically concentrated GPU collateral.

---

## Tier 2 Reserved-Capacity Index (CRI-R2)

**Target:** TBD (contingent on API access or data-sharing agreements)
**Methodology version:** Separate document — METHODOLOGY-CRI-R2-v1.0.md

### What It Measures
CRI-H100 measures Tier 3 (on-demand secondary marketplace) pricing. CRI-R2 would measure Tier 2 (reserved and committed capacity from mid-market providers) — closer to the pricing environment of mid-market GPU-backed borrowers.

### Data Requirements
API access to published reserved-instance rate cards from multiple providers, or data-sharing agreements with mid-market platforms (Lambda, CoreWeave spot, RunPod reserved).

### Credit Application
Most GPU-backed credit agreements involve borrowers whose revenue comes from Tier 2 reserved capacity, not Tier 3 spot. CRI-R2 would provide a more directly applicable reference rate for this borrower profile. CRI-H100 would remain useful as a stress-case floor.

---

## What Will Not Change

The following principles are permanent features of all CRI indices and are not subject to extension or modification:

- **Open methodology:** All calculation steps published and independently reproducible
- **No authentication required:** Raw data accessible without licensing or API keys
- **IOSCO governance:** All indices governed under CCIR Governance Framework
- **Free citation:** Credit documents may reference CRI indices without a licensing fee
- **Append-only audit trail:** SHA-256 tamper-evident data archive
- **Semantic versioning:** All methodology changes follow the versioning policy in VERSIONING.md

---

© 2026 Compute Credit Index Research LLC · CC BY 4.0
