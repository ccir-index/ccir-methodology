# CCIR Methodology v1.0.0.0
## CRI-H100: Daily H100 SXM GPU Rental Rate Index

**Published by:** Compute Credit Index Research LLC (CCIR)
**Effective date:** March 10, 2026
**First publication:** March 10, 2026
**Governance document:** [GOVERNANCE-v1_0.md](./GOVERNANCE-v1_0.md)
**Repository:** https://github.com/ccir-index/ccir-methodology
**Contact:** research@ccir.io

---

## Version History

| Version | Date | Change Summary | Material? |
|---------|------|----------------|-----------|
| v1.0.0 | March 10, 2026 | Initial public release. Daily publication. Five live indices (CRI-H100, CRI-A100, CRI-H200, CRI-L40S, CRI-B200). Multi-source (Vast.ai, Shadeform, RunPod). Median-of-medians calculation. | N/A — first release |

For the complete versioning policy — including what constitutes a material change and how existing contracts are affected — see [VERSIONING.md](./VERSIONING.md).

---

## 1. Purpose and Scope

CRI-H100 is a daily index measuring the trailing 7-day median $/GPU-hour for NVIDIA H100 SXM on-demand rental listings in the US marketplace. It is designed for:

- Reference citation in credit agreements and loan covenants
- Collateral valuation benchmarking in GPU-backed lending
- Covenant trigger design (minimum rental rate floor, cash sweep, appraisal event)
- ABS structuring and rating agency collateral analysis

CRI-H100 is governed by the CCIR Governance Framework v1.0.0. The governance document defines the Administrator, Calculation Agent, Oversight Board, change control process, fallback waterfall, complaints procedure, and IOSCO Principles alignment. This methodology document defines the calculation. Together they constitute the complete specification.

---

## 2. Formal Definitions

The following terms have specific meanings throughout this document and in any credit agreement referencing CRI-H100.

**Raw Listing.** A single entry returned by a source API for a GPU provider instance at the time of collection. Each raw listing corresponds to one provider's offer to rent their hardware.

**Qualifying Listing.** A Raw Listing that satisfies all Quality Filters defined in Section 5. Only Qualifying Listings are used in index calculation.

**Observation.** The per-GPU-hour price derived from a Qualifying Listing: `price_per_gpu_hr = total_price / num_gpus`. This is the unit of analysis for all calculation steps.

**Snapshot.** The complete set of Raw Listings retrieved from all Sources on a single Collection Day. Each Snapshot is archived with a SHA-256 hash at the time of collection.

**Collection Day.** A calendar day on which CCIR executes the data collection pipeline. Collection runs twice daily on an automated schedule: a Primary Collection at 09:00 CT and a Fallback Collection at 21:00 CT. The daily index calculation uses Primary Collection data; Fallback Collection data is promoted automatically if Primary Collection data is absent for a given source on a given day.

**Valid Day.** A Collection Day that produces at least 2 contributing Sources with qualifying observations for a given model. Only Valid Days contribute to the Confidence Flag calculation.

**Calculation Window.** The trailing 7-calendar-day period ending on the current Publication Date.

**Publication Date.** Each calendar day. CRI-H100 is published daily by automated pipeline following collection.

**Per-Source Median.** The median Observation price across all Qualifying Listings from a single Source within the Calculation Window. Each Source produces exactly one Per-Source Median per Publication Date.

**Index Value.** The median of all Per-Source Medians. Each Source receives one vote regardless of its listing count. This median-of-medians architecture limits the influence of any single source.

**Excluded Listing.** A Raw Listing that fails one or more Quality Filters, or an Observation removed by the outlier removal procedure. Excluded Listings are not used in calculation but are retained in the archive.

**Confidence Flag.** A disclosure applied to each daily publication indicating statistical reliability. See Section 7 for trigger conditions and interpretation.

**Administrator.** Compute Credit Index Research LLC, as defined in Governance Framework §2.1.

**Calculation Agent.** The party responsible for executing the daily data collection and index calculation. Currently CCIR. May be delegated subject to Governance Framework §3.2.

**Manifest Error.** An obvious, verifiable error in a published value (e.g., a decimal place error, data file corruption, or API malfunction producing clearly anomalous values). See Governance Framework §8.

**Restatement.** A correction to a previously published value following identification of a Manifest Error. Subject to Governance Framework §8.3.

---

## 3. What CRI-H100 Measures — and Does Not Measure

### 3.1 What It Measures

CRI-H100 is an **on-demand marginal supply rate** for secondary marketplace GPU compute capacity. It measures the price at which providers on public marketplaces are willing to supply H100 SXM GPU-hours to any buyer without a pre-existing contract.

CRI-H100 measures:
- Listed rental rates (willingness to supply), not executed transaction prices
- On-demand capacity, not reserved or committed capacity
- Tier 3 secondary marketplace pricing, not Tier 1 enterprise contracts
- US geography only (v1.0.0 scope)
- H100 SXM model specifically, not H100 PCIe or other variants

### 3.2 What It Does Not Measure

CRI-H100 does **not** measure:
- Enterprise bilateral contracts between neoclouds and hyperscalers
- Volume-weighted clearing rates
- Executed transaction prices (only listed prices)
- Non-US rental rates
- H100 PCIe, H200, A100, B200, or other GPU models (separate indices)
- Performance-adjusted compute value
- Reserved-capacity or committed-instance pricing (covered by CRI-R companion series)

### 3.3 Appropriate Use in Credit Agreements

**(a) Stress-case floor.** For borrowers with Tier 1 enterprise contracts, CRI-H100 approximates the rate they would face if they lost an anchor contract and had to re-lease on the open market.

**(b) Direct reference rate.** For borrowers whose revenue derives primarily from Tier 2 or Tier 3 marketplace capacity.

**(c) Leading indicator.** Sustained CRI-H100 declines signal that uncommitted capacity is becoming cheaper, which pressures enterprise contract renewal rates on a lagged basis.

Lenders should calibrate covenant trigger levels to account for the spread between enterprise rates and Tier 3 spot rates. That spread is tracked by the CRI-R Companion Series.

---

## 4. Data Sources

### 4.1 Source Inventory

CRI-H100 v1.0.0 is calculated from listings retrieved across three public sources:

| Source ID | Provider | Endpoint | Auth Required | Authentication |
|-----------|----------|----------|---------------|----------------|
| VASTAI_SPOT | Vast.ai | `console.vast.ai/api/v0/bundles/` | None | Public — no key required |
| SHADEFORM_MKT | Shadeform | `api.shadeform.ai/v1/instances/types` | Free API key | Free registration |
| RUNPOD_SPOT | RunPod | `api.runpod.io/graphql` | Free API key | Free registration |

All three sources are publicly accessible. Any counterparty can independently query these endpoints and reproduce CCIR's inputs without a data licensing agreement.

### 4.2 Source Selection Criteria

Sources are selected on three criteria:

**Reproducibility.** The source must be publicly accessible. Any third party must be able to independently reproduce the index without any proprietary data relationship.

**Transparency.** Listing data must include sufficient metadata to apply verifiable quality filters: model identification, geographic information, and pricing.

**Market representation.** The source must represent active spot market pricing for H100 SXM compute in the US.

### 4.3 Multi-Source Architecture and Manipulation Resistance

The median-of-medians calculation assigns each qualifying source exactly one vote in the final index value. A source with 1,000 listings has no more influence than a source with 10 listings. This design limits the ability of any single marketplace participant to move the index by flooding one source with anomalous listings.

Adding any new source as an official CRI-H100 input requires a Non-Material Methodology Change with 14-day advance notice per Governance Framework §5.3.

### 4.4 Source Registry Maintenance

The Source Registry is reviewed on a monthly basis per Governance Framework §10.5. Reviews cover:

- GPU model nomenclature changes (new SKU names, model variants)
- Tier classification accuracy (spot vs. reserved vs. on-demand)
- New GPU generation detection (new model families entering marketplace supply)
- Migration pattern monitoring (shifts in listing volume across generations)
- API endpoint and schema changes

Changes identified during review are classified as Material or Non-Material per Section 5 and disclosed accordingly.

---

## 5. Quality Filters

The following filters are applied to all raw listings. A listing must satisfy all filters to become a Qualifying Listing.

| Filter | Specification | Rationale |
|--------|---------------|-----------|
| Hardware model | H100 SXM only (canonical: `NVIDIA H100 SXM5 80GB`) | Intra-model variance requires strict model identification |
| Geography | US region only (`region_normalized = 'us'`) | Geographic scope defined in Section 8 |
| Availability | Active listing, unrented at time of collection | Rented listings do not represent available supply |
| Outlier removal | IQR ± 3× fence, applied per source per collection day | Removes anomalous listings without eliminating legitimate price dispersion |
| Price floor | Price > $0.00/GPU-hour | Eliminates data errors |

Outlier fences are computed per-source per-day: Q1 − 3×IQR (lower) and Q3 + 3×IQR (upper). Listings outside these fences are flagged `is_outlier = True` and excluded from index calculation.

---

## 6. Calculation Procedure

The index is calculated by a deterministic, rules-based pipeline. No discretionary inputs are applied in routine calculation.

### Step 1 — Collect
Automated hourly collection from all three Sources. Each raw listing is written to the Bronze Delta Lake table with full provenance fields (source_id, collected_at, methodology_version, raw_payload, SHA-256).

### Step 2 — Filter and Normalize
Quality filters applied per Section 5. Each listing is normalized to per-GPU-hour pricing: `price_per_gpu_hr = price / num_gpus`. Model lookup normalizes raw GPU name strings to canonical model identifiers.

### Step 3 — Per-Source Median
For each qualifying source, compute the median price across all Qualifying Listings within the 7-day Calculation Window:

```
per_source_median(s) = median({ observation_i : source(i) = s, i ∈ QualifyingListings })
```

### Step 4 — Median of Medians (Index Value)
Compute the median of all per-source medians:

```
CRI-H100 = median({ per_source_median(s) : s ∈ ContributingSources })
```

Each contributing source receives exactly one vote. The number of qualifying listings within a source does not affect its weight in the final calculation.

### Step 5 — Confidence Flag
Assign confidence flag per Section 7.

### Step 6 — Publish
Append Gold row to `cri-h100/CRI-H100-history.csv` in the ccir-data repository. Commit with SHA-256 provenance.

---

## 7. Confidence Flags and Publication Standards

### 7.1 Confidence Flag Thresholds

| Flag | Condition |
|------|-----------|
| `high` | ≥ 3 contributing sources AND ≥ 5 Valid Days in the window |
| `medium` | ≥ 2 contributing sources AND ≥ 3 Valid Days in the window |
| `low` | All other cases |

### 7.2 Low Confidence Flag

A Low Confidence publication is never suppressed. A directional estimate with disclosed uncertainty is more useful to credit markets than a data gap.

Lenders referencing CRI-H100 in covenant documents should incorporate the confidence regime into trigger design. A trigger set at a 5% decline is premature when the confidence interval reflects limited observation depth.

### 7.3 Publication Record Integrity

The published series is **append-only**. Once a value is published, it is not revised except under Restatement provisions (Governance Framework §8.3). Each daily publication is committed to the public repository with a Git SHA providing an immutable, time-stamped record.

Each collection includes a SHA-256 hash of the raw data committed at collection time, creating a tamper-evident audit trail from every published index value to its underlying raw data.

---

## 8. Geographic Scope

CRI-H100 v1.0.0 covers **United States listings only**.

Geographic normalization maps raw region strings to `us`, `eu`, `asia`, or `other`. Only `us` rows are included in the index calculation. Source-specific overrides apply where APIs return non-geographic region identifiers (e.g., RunPod's `runpod_global` is normalized to `us`).

Regional expansion (CRI-H100-EU, CRI-H100-APAC) is planned under identical methodology with geography-only filter changes.

---

## 9. Fallback Procedures

The following is an abbreviated summary. The complete Fallback Waterfall is in Governance Framework §7.

| Trigger | Response |
|---------|----------|
| Only 1 source available for < 2 Business Days | Publish with Low Confidence flag; note source unavailability in publication |
| Only 1 source available for 2–5 Business Days | Publish with Low Confidence flag; notify known users within 2 Business Days |
| All sources unavailable > 5 Business Days | Activate Fallback Level 2: secondary marketplace data |
| Source count drops to 1 for > 5 consecutive days | Initiate source review; escalate to Oversight |
| Manifest Error identified | Correct via Restatement (Governance §8.3); publish correction notice |

---

## 10. Known Limitations

### 10.1 Listed Prices vs. Executed Prices
CRI-H100 measures listed rental rates, not executed transaction prices. The index reflects willingness to supply, not confirmed transactions. Executed transaction data requires proprietary relationships incompatible with the open-methodology requirement.

### 10.2 Marketplace Scope
CRI-H100 v1.0.0 is sourced from three public spot marketplaces. Enterprise bilateral contracts, hyperscaler pricing, and other marketplace rates are not reflected. The three sources collectively represent a reproducible proxy for on-demand spot pricing but do not capture the full market.

### 10.3 Thin Observation Counts
Early-period daily observation counts are substantially below the data depth of established commodity benchmarks. The Confidence Flag and observation count disclosures are the mechanism for communicating this limitation.

### 10.4 Intra-Model Performance Variance
CRI-H100 does not adjust for performance variance within the H100 SXM model. Two listings at identical $/GPU-hour may represent economically different compute products. A performance-adjusted variant (CRI-H100-PA) is on the roadmap.

### 10.5 Staleness of Listed Prices
Some listed prices may reflect outdated market conditions. The 7-day active listing filter mitigates but does not eliminate this.

### 10.6 No Volume Weighting
CRI-H100 uses the unweighted per-source median. Volume-weighting is not applied because large-listing volume may reflect inventory concentration rather than market-clearing prices. The median is more robust for credit reference purposes.

### 10.7 Geographic Approximation
Region fields in source APIs reflect provider-registered data center location, not necessarily physical hardware location. Edge cases may exist where hardware is misclassified geographically.

---

## 11. Governance Cross-References

| Methodology Topic | Governance Section |
|------------------|--------------------|
| Administrator and Calculation Agent roles | §2 |
| Data Input Design (no-submissions architecture) | §2.4 |
| Source Registry Maintenance | §10.5 |
| Fallback Waterfall (4 levels) | §7 |
| Benchmark Replacement Waterfall | §7.4 |
| Change Control (semantic versioning, notice periods) | §5 |
| Manifest Error and Restatement | §8 |
| Annual Benchmark Review | §4.3 |
| Independent Audit commitment | §11 |
| Complaints Procedure | §12 |
| Rating Agency Disclosure | §15 |
| IOSCO Principles Alignment | Appendix C |

In the event of conflict between this methodology document and the Governance Framework, the Governance Framework governs with respect to process, and the Methodology governs with respect to calculation.

---

## 12. Independent Reproducibility

Any party can independently reproduce any published CRI-H100 value. The full process is documented in [REPRODUCIBILITY.md](./REPRODUCIBILITY.md).

The verification process:
1. Clone the ccir-data repository
2. Verify SHA-256 hashes on all raw collection files for the calculation window
3. Apply the quality filters and normalization per this document
4. Compute per-source medians, then median-of-medians
5. Compare result to the published value

Any discrepancy beyond floating-point rounding is grounds for a formal complaint per Governance Framework §12.

---

## 13. Companion Indices

CCIR collects data for multiple GPU models daily. The following indices are currently published:

| Index | Model | Status | Primary Use Signal |
|-------|-------|--------|--------------------|
| CRI-B200 | NVIDIA B200 SXM 192GB | Live — March 10, 2026 | Frontier training; next-gen displacement signal |
| CRI-H200 | NVIDIA H200 SXM5 141GB | Live — March 10, 2026 | Large-model training and memory-bound inference |
| CRI-H100 | NVIDIA H100 SXM5 80GB | Live — March 10, 2026 | Current training tier; primary collateral reference rate |
| CRI-A100 | NVIDIA A100 SXM4 80GB | Live — March 10, 2026 | Inference and batch workloads; cascade velocity signal |
| CRI-L40S | NVIDIA L40S 48GB | Live — March 10, 2026 | Inference-optimized; commodity cascade pricing |

All five indices are published as append-only CSV series in the ccir-data repository. Each shares an identical methodology, schema, and governance framework. The CRI-R Companion Series (hyperscaler and mid-market rate cards) is published alongside the spot indices as contextual reference data.

---

*This document is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Any party may freely cite, reference, or reproduce this methodology with attribution.*

*© 2026 Compute Credit Index Research LLC*
