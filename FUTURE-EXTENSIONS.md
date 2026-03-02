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

CRI-H100-PA will normalize the raw $/GPU-hour price by a standardized measure of delivered compute, enabling cross-hardware comparison while preserving the transparency and reproducibility of rental-rate data. In formal terms:

```
PA-Rate = Rental Rate / Performance Index
```

where the Performance Index is a dimensionless composite score representing the GPU's effective compute capacity — not its theoretical FLOP ceiling, but its observed, reproducible throughput under standard workloads.

### Role in the Index Family
CRI-H100-PA occupies a specific and distinct position in the CRI index family:

| Index | Answers | Primary Use |
|-------|---------|-------------|
| CRI-H100 | What does the market charge per GPU-hour? | Credit agreements, covenant triggers, collateral valuation |
| CRI-H100-PA | What does the market charge per unit of delivered compute? | Cross-hardware comparison, procurement, depreciation modeling |
| CRI-D | How fast are rental rates declining across generations? | LGD modeling, ABS stress testing |
| CRI-S | Is the liquidation floor holding relative to rental rates? | Collateral coverage monitoring |

CRI-H100 is the cash-flow benchmark. CRI-H100-PA is the compute-normalized benchmark. They are complements, not substitutes.

### The Bridge to a Compute Standard

CRI-H100-PA is designed as the methodological bridge between hardware-specific rental rates and a future cross-hardware compute standard of the kind described in the Trump Administration's AI Action Plan (July 2025) and the academic literature on compute market infrastructure.

The central challenge in building compute derivatives — raised by analysts including Dave Friedman in "The Compute Market Is Building in the Wrong Order" (March 2026) — is the absence of a dimensionless compute unit that survives GPU generation transitions. Without such a unit, forward contracts on H100 GPU-hours face embedded obsolescence risk every 18–24 months, because there is no agreed conversion ratio between an H100-hour and a B200-hour.

A performance-adjusted rental rate addresses exactly this gap. By expressing rental cost in terms of a standardized compute unit rather than a specific GPU model, CRI-H100-PA produces a price series that can, in principle, be extended across hardware generations as the Performance Index is recalibrated for each new architecture. This is not yet a full cross-hardware standard — but it is the empirical foundation one would require.

CCIR's position is that this sequencing matters: hardware-specific rental rates first (CRI-H100), performance-adjusted rates second (CRI-H100-PA), and cross-hardware standardization third — when the institutional infrastructure exists to support it. The historical rental rate time series CRI-H100 is accumulating is itself a prerequisite for any future standardization process: it provides the empirical price record against which a compute unit would need to be anchored to real market values. SOFR was not built from scratch; it was built on top of existing repo transaction data. A compute standard will similarly require a credible historical price series.

CCIR will design CRI-H100-PA to be compatible with a future NIST or equivalent compute standard if and when one emerges, without committing to any specific institutional timeline.

### Performance Index Design

A credible Performance Index must reflect delivered compute, not theoretical FLOP ceilings. For modern accelerators, this means a weighted composite of:

- **Tensor throughput** (FP8/FP16/BF16) — primary signal for training workloads
- **Memory bandwidth** — critical for inference and large-model serving
- **Interconnect bandwidth** (NVLink, PCIe, NVSwitch) — relevant for multi-GPU configurations
- **Model-level performance** — LLM tokens/sec or training throughput under standard benchmark conditions

Weights may be equal (maximally transparent), workload-specific (LLM-optimized, training-optimized), or empirically derived (regression against observed rental prices).

### Normalization Options Under Evaluation

Three approaches are under evaluation, in increasing order of ambition:

**Option 1 — CCIR-defined Performance Index.** CCIR publishes a composite score under the same open-methodology governance as CRI-H100. Most transparent and governable; best aligned with IOSCO principles. Requires CCIR to run or commission a reproducible benchmark suite on sampled providers.

**Option 2 — Industry benchmark normalization.** Normalize against an established benchmark suite (MLPerf, HuggingFace Open LLM Leaderboard, or equivalent). More workload-realistic, but benchmarks evolve and may be harder to govern over multi-year periods. Introduces a dependency on a third-party benchmark standard.

**Option 3 — Future NIST compute unit.** If NIST or an equivalent body produces a standardized compute unit as envisioned in the AI Action Plan, CRI-H100-PA would adopt it as the denominator. This is the ideal long-term outcome but cannot be committed to on any specific timeline.

CCIR's current preference is Option 1 as the launch methodology, with explicit compatibility design for Option 3.

### Data Requirements
- Minimum 26 weekly CRI-H100 observations (six months) to establish a meaningful PA baseline
- Reproducible benchmark scores for a representative sample of Vast.ai H100 SXM providers
- Governance framework for Performance Index updates as hardware configurations evolve

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
