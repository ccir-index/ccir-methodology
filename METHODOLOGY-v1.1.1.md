# CCIR Methodology v1.1.1
## CRI-H100: Weekly H100 SXM GPU Rental Rate Index

**Published by:** Compute Credit Index Research LLC (CCIR)
**Effective date:** February 2026
**Supersedes:** v1.0.0 (internal pre-publication draft — not publicly distributed)
**Governance document:** [GOVERNANCE-v1_0.md](./GOVERNANCE-v1_0.md)
**Repository:** https://github.com/ccir-index/ccir-methodology
**Contact:** research@ccir.io

---

## Version History

| Version | Date | Change Summary | Material? | Notice Required |
|---------|------|----------------|-----------|-----------------|
| v1.1.1 | Feb 2026 | Initial publication. Multi-model collection, formalized Low Confidence thresholds. | No | N/A |
| v1.2.0 | TBD | Planned: CRI-A100 formal publication under identical methodology | No | 14 days |
| v2.0.0 | TBD | Planned: CRI-S multi-source aggregation framework | Yes | 60 days |

For the complete versioning policy — including what constitutes a material change and how existing contracts are affected — see [VERSIONING.md](./VERSIONING.md).

---

## 1. Purpose and Scope

CRI-H100 is a weekly index measuring the trailing 7-day median $/GPU-hour for NVIDIA H100 SXM on-demand rental listings in the US marketplace. It is designed for:

- Reference citation in credit agreements and loan covenants
- Collateral valuation benchmarking in GPU-backed lending
- Covenant trigger design (minimum rental rate floor, cash sweep, appraisal event)
- ABS structuring and rating agency collateral analysis

CRI-H100 is governed by the CCIR Governance Framework v1.0. The governance document defines the Administrator, Calculation Agent, Oversight Board, change control process, fallback waterfall, complaints procedure, and IOSCO Principles alignment. This methodology document defines the calculation. Together they constitute the complete specification.

---

## 2. Formal Definitions

The following terms have specific meanings throughout this document and in any credit agreement referencing CRI-H100.

**Raw Listing.** A single entry returned by the Vast.ai API for a GPU provider instance at the time of daily collection. Each raw listing corresponds to one provider's offer to rent their hardware.

**Qualifying Listing.** A Raw Listing that satisfies all Quality Filters defined in Section 5. Only Qualifying Listings are used in index calculation.

**Observation.** The per-GPU-hour price derived from a Qualifying Listing: `Observation = dph_total / num_gpus`. This is the unit of analysis for all calculation steps.

**Snapshot.** The complete set of Raw Listings retrieved from the Vast.ai API on a single Collection Day. Each Snapshot is archived with a SHA-256 hash at the time of collection.

**Collection Day.** A calendar day on which CCIR executes the data collection pipeline. The target collection time is 9:00 AM Central Time. Minor deviations in collection time do not affect the index calculation.

**Valid Day.** A Collection Day that produces at least 8 Qualifying Listings after all Quality Filters are applied. Only Valid Days contribute Observations to the weekly calculation.

**Calculation Window.** The trailing 7-calendar-day period ending on the Wednesday immediately preceding each Thursday Publication Date.

**Publication Date.** The Thursday of each calendar week. CRI-H100 is published by 5:00 PM Central Time on each Publication Date.

**Trimmed Mean.** The arithmetic mean of the middle 80% of Observations (after removing the top 10% and bottom 10% by value), computed for the purpose of outlier removal only. The Trimmed Mean is not published and is not the index value.

**Excluded Listing.** A Raw Listing that fails one or more Quality Filters, or an Observation that is removed by the outlier removal procedure. Excluded Listings and Observations are not used in calculation but are retained in the archive.

**Low Confidence Flag.** A disclosure applied to a weekly publication when the statistical precision of the index value is below the threshold for mechanical covenant reference. See Section 7 for trigger conditions and interpretation.

**Administrator.** Compute Credit Index Research LLC, as defined in Governance Framework §2.1. The Administrator oversees the index, maintains governance, and is responsible for all publications.

**Calculation Agent.** The party responsible for executing the daily data collection and weekly index calculation. Currently CCIR. May be delegated to a third party subject to Governance Framework §3.2.

**Manifest Error.** An obvious, verifiable error in a published value (e.g., a decimal place error, data file corruption, or API malfunction producing clearly anomalous values). Distinguished from a market movement or data quality issue. See Governance Framework §8.

**Restatement.** A correction to a previously published value following identification of a Manifest Error. Subject to the Restatement provisions of Governance Framework §8.3. Restated values are clearly labeled in the published series.

---

## 3. What CRI-H100 Measures — and Does Not Measure

### 3.1 What It Measures

CRI-H100 is an **on-demand marginal supply rate** for secondary marketplace GPU compute capacity. It measures the price at which providers on a public marketplace are willing to supply H100 SXM GPU-hours to any buyer without a pre-existing contract. It is structurally analogous to an electricity spot market price or a posted dealer offer rate.

CRI-H100 measures:
- Listed rental rates (willingness to supply), not executed transaction prices
- On-demand capacity, not reserved or committed capacity
- Tier 3 secondary marketplace pricing (Vast.ai), not Tier 1 enterprise contracts
- US geography only (v1.1.1 scope)
- H100 SXM model specifically, not H100 PCIe or other variants

### 3.2 What It Does Not Measure

CRI-H100 does **not** measure:
- Enterprise bilateral contracts between neoclouds and hyperscalers
- Volume-weighted clearing rates
- Executed transaction prices (only listed prices)
- Non-US rental rates
- H100 PCIe, H200, A100, or other GPU models (separate indices)
- Performance-adjusted compute value (see CRI-H100-PA roadmap)
- Reserved-capacity or committed-instance pricing (see CRI-R2 roadmap)

### 3.3 Appropriate Use in Credit Agreements

CRI-H100 is most appropriately used as:

**(a) Stress-case floor.** For borrowers with Tier 1 enterprise contracts, CRI-H100 approximates the rate they would face if they lost an anchor contract and had to re-lease on the open market.

**(b) Direct reference rate.** For borrowers whose revenue derives primarily from Tier 2 or Tier 3 marketplace capacity.

**(c) Leading indicator.** Sustained CRI-H100 declines signal that uncommitted capacity is becoming cheaper, which pressures enterprise contract renewal rates on a lagged basis.

Using CRI-H100 to underwrite a facility whose cash flows derive entirely from Tier 1 enterprise contracts without appropriate haircuts would be a structural mismatch. Lenders should calibrate covenant trigger levels to account for the spread between enterprise rates and Tier 3 spot rates.

---

## 4. Data Source

### 4.1 Source Description

CRI-H100 is calculated from listings retrieved from the Vast.ai GPU rental marketplace public API.

**Endpoint:** `https://console.vast.ai/api/v0/bundles/`
**Authentication:** None required
**Query structure:** Per-model targeted queries (see Section 4.3)

### 4.2 Source Selection Criteria

Vast.ai was selected on three criteria:

**Reproducibility.** The API requires no authentication. Any counterparty, attorney, rating agency, or third party can independently query the same endpoint and reproduce CCIR's inputs without any data licensing agreement. This is the foundational requirement for a credit reference rate in a post-LIBOR environment.

**Transparency.** Listing data includes provider reliability scores, geographic information, hardware specifications, and pricing — sufficient metadata to apply meaningful, verifiable quality filters.

**Market representation.** Vast.ai is among the largest public GPU rental marketplaces by listing volume, providing the most reproducible proxy available for on-demand spot market pricing.

### 4.3 Query Architecture

The Vast.ai API caps broad queries at 64 results regardless of actual market depth. To ensure complete coverage, CCIR executes separate targeted queries for each GPU model:

```
GET https://console.vast.ai/api/v0/bundles/?q={"gpu_name":{"eq":"H100 SXM"},"rentable":{"eq":true},"rented":{"eq":false},"reliability2":{"gte":0.9},"num_gpus":{"gte":1},"type":"on-demand"}
```

Per-model queries are executed for all 9 models in the collection pipeline. H100 SXM is the primary index model. Other models are collected for depreciation curve analysis (Stage 2).

### 4.4 Third-Party Dependency Risk

Governance Framework §9 addresses Vast.ai as a Third Party Data Provider. Trigger events for data source review include:

- API unavailability for more than 2 consecutive Business Days
- API structural changes that prevent execution of the standard query
- Observation count declining below 5 Qualifying Listings per day for more than 10 consecutive Collection Days

In any of these events, the Fallback Waterfall (Governance Framework §7) governs the response. See Section 9 of this document for an abbreviated summary.

---

## 5. Quality Filters

All filters are applied sequentially to each Raw Listing. A listing must satisfy **all** filters to become a Qualifying Listing. Listings failing any filter are archived as Excluded Listings with the reason for exclusion recorded.

| Filter | Field | Criterion | Rationale |
|--------|-------|-----------|-----------|
| Hardware | `gpu_name` | equals `"H100 SXM"` | Consistent hardware specification |
| Availability — rentable | `rentable` | equals `true` | Provider is accepting rental requests |
| Availability — not rented | `rented` | equals `false` | Currently available at listed price |
| Reliability | `reliability2` | greater than or equal to `0.90` | Excludes unreliable providers |
| Minimum GPU count | `num_gpus` | greater than or equal to `1` | Excludes malformed listings |
| Staleness | `start_date` | within 7 calendar days of collection date | Excludes stale listings not reflecting current market |
| Geography | `geolocation` | ends with `", US"` | US-only scope |

### 5.1 Filter Rationale Detail

**Reliability filter (≥ 0.90).** The `reliability2` score reflects the provider's historical uptime and fulfillment rate as tracked by Vast.ai. Providers below 0.90 are excluded because their listings may not represent genuine, fillable capacity — a lender or borrower relying on such a listing as a market reference would be using a non-executable quote. The 0.90 threshold also creates a meaningful economic barrier to index manipulation: creating a qualifying provider account requires sustained operation with genuine rental activity over multiple weeks.

**Staleness filter (7 days).** Listings with a `start_date` older than 7 days are excluded as potentially stale. A listing that has been on the market for more than 7 days without being rented may reflect an uncompetitive price, a dysfunctional provider, or a listing that has not been actively maintained. Including stale listings would introduce upward bias.

**Geography filter.** Vast.ai's `geolocation` field contains the provider's data center location. Filtering for entries ending with `", US"` restricts to continental US listings. This geographic scope was selected because the majority of GPU-backed credit agreements are governed by US law and US rental rates may reflect different supply-demand dynamics than non-US markets.

---

## 6. Calculation

### 6.1 Pseudocode

The following pseudocode defines the CRI-H100 calculation. The implementation in `pipeline/calculate.py` follows this specification exactly.

```
INPUT: collection_window = trailing 7 calendar days ending Wednesday

valid_days = []
pooled_observations = []

FOR each day IN collection_window:

    daily_snapshot = load_snapshot(day)   # data/raw/YYYY-MM-DD.csv

    # Step 1 — Apply quality filters
    qualifying = []
    FOR each listing IN daily_snapshot:
        IF listing.gpu_name != "H100 SXM":    SKIP
        IF listing.rentable != true:           SKIP
        IF listing.rented != false:            SKIP
        IF listing.reliability2 < 0.90:        SKIP
        IF listing.num_gpus < 1:              SKIP
        IF listing.start_date < (day - 7):    SKIP
        IF NOT listing.geolocation.endswith(", US"): SKIP
        observation = listing.dph_total / listing.num_gpus
        qualifying.append(observation)

    IF len(qualifying) < 8:
        mark_day_as_excluded(day, reason="insufficient observations")
        CONTINUE

    # Step 2 — Outlier removal
    sorted_obs = sort(qualifying)
    trim_n = floor(len(sorted_obs) * 0.10)
    trimmed = sorted_obs[trim_n : len(sorted_obs) - trim_n]
    trimmed_mean = mean(trimmed)
    stdev = standard_deviation(qualifying)   # full set, not trimmed
    threshold = 2.5 * stdev

    cleaned = []
    FOR each obs IN qualifying:
        IF abs(obs - trimmed_mean) <= threshold:
            cleaned.append(obs)

    mark_day_as_valid(day)
    valid_days.append(day)
    pooled_observations.extend(cleaned)

# Step 3 — Weekly aggregation
IF len(valid_days) >= 3:
    cri_h100 = median(pooled_observations)
    low_confidence = (len(valid_days) < 7) OR (any_day_had_fewer_than_8_obs)
ELSE:
    cri_h100 = median(pooled_observations)   # publish anyway with Low Confidence flag
    low_confidence = true

# Step 4 — Publication record
OUTPUT:
    index_value      = round(cri_h100, 4)
    valid_days       = len(valid_days)
    total_obs        = len(pooled_observations)
    min_obs          = min(pooled_observations)
    max_obs          = max(pooled_observations)
    mean_obs         = mean(pooled_observations)
    stdev_obs        = standard_deviation(pooled_observations)
    ci_95_lower      = bootstrap_percentile(pooled_observations, 2.5)
    ci_95_upper      = bootstrap_percentile(pooled_observations, 97.5)
    low_confidence   = low_confidence
    methodology_ver  = "v1.1.1"
```

### 6.2 Central Tendency: Why the Median

The median was selected over alternative measures for three reasons:

**(a) Manipulation resistance.** Moving the median requires controlling more than half of all Qualifying Listings in the calculation window. This requires a substantially higher capital outlay than moving a mean, where a small number of anomalous listings can have disproportionate influence.

**(b) No volume weighting required.** The median requires no transaction volume data, which is not reliably available from public marketplace APIs. Volume-weighted measures introduce unknown bias from unreliable volume fields.

**(c) Credit appropriateness.** For loan covenant purposes, a robust central tendency measure is more appropriate than a volume-weighted measure that could be influenced by large-listing behavior.

### 6.3 Outlier Removal: The 2.5σ Threshold

The 2.5σ threshold balances two competing objectives: removing genuinely anomalous listings without excluding legitimate price dispersion.

A tighter threshold (e.g., 2.0σ) risks removing valid high-price or low-price listings that represent real market segments. A looser threshold (e.g., 3.0σ) permits erroneous or manipulative listings to influence the index. The 2.5σ threshold was selected as the midpoint that preserves the natural dispersion observed in GPU rental markets while excluding outliers that deviate materially from the observable distribution.

The outlier removal procedure applies to each daily dataset independently, not to the pooled weekly dataset. This prevents a single anomalous day from contaminating other days' observations.

### 6.4 Bootstrap Confidence Intervals

For each weekly publication, CCIR computes a nonparametric 95% bootstrap confidence interval:

```
B = 10,000 bootstrap resamples of size N (with replacement)
for each resample: compute median
CI_95 = [2.5th percentile of bootstrap medians, 97.5th percentile]
```

The confidence interval is published alongside the index value and reflects the statistical precision achievable given the observed sample size. In thin observation regimes, the interval is wide — this is disclosed, not suppressed.

| Weekly Observations (N) | Expected 95% CI Width | CCIR Classification |
|------------------------|----------------------|---------------------|
| < 20 | ~15–25%+ of median | Low Confidence |
| 20–40 | ~8–15% of median | Standard |
| 40–80 | ~4–8% of median | Standard |
| 80+ | < 4% of median | High Confidence |

---

## 7. Publication

### 7.1 Publication Schedule

CRI-H100 is published weekly on **Thursdays by 5:00 PM Central Time**.

Each publication includes:

| Field | Description |
|-------|-------------|
| `publication_date` | Thursday publication date (YYYY-MM-DD) |
| `calculation_window_start` | First day of trailing 7-day window |
| `calculation_window_end` | Wednesday preceding publication |
| `cri_h100` | Index value, rounded to 4 decimal places |
| `valid_days` | Number of Valid Days in calculation window |
| `total_observations` | Total pooled Observations after outlier removal |
| `min` | Minimum Observation in pool |
| `max` | Maximum Observation in pool |
| `mean` | Arithmetic mean of pool |
| `stdev` | Standard deviation of pool |
| `ci_95_lower` | Lower bound of 95% bootstrap confidence interval |
| `ci_95_upper` | Upper bound of 95% bootstrap confidence interval |
| `low_confidence` | Boolean — true if Low Confidence flag applies |
| `methodology_version` | `"v1.1.1"` |

### 7.2 Low Confidence Flag

The Low Confidence flag is applied when **any** of the following conditions is met:

- Any single Collection Day in the window had fewer than 8 Qualifying Listings
- Fewer than 3 Valid Days exist in the 7-day window
- The 95% bootstrap confidence interval exceeds 25% of the published median

A Low Confidence publication is not suppressed. A directional estimate with disclosed uncertainty is more useful to credit markets than a data gap. Lenders referencing CRI-H100 in covenant documents should incorporate the confidence regime into trigger design: a cash sweep trigger set at a 5% decline is premature when the confidence interval is ±20%.

### 7.3 Publication Record Integrity

The published series is **append-only**. Once a value is published, it is not revised except under Restatement provisions (Governance Framework §8.3). Each weekly publication is committed to the public repository with a Git SHA that provides an immutable, time-stamped record.

Each daily data collection includes a SHA-256 hash of the raw data file committed at the time of collection. This creates a tamper-evident audit trail linking every published index value to its underlying raw data.

---

## 8. Geographic Scope

CRI-H100 v1.1.1 covers **United States listings only**.

Geographic scope is enforced by filtering for `geolocation` values ending with `", US"`. This captures continental US data centers as represented in Vast.ai's location field.

Regional expansion (CRI-H100-EU, CRI-H100-APAC) is planned under identical methodology with geography-only filter changes. See [FUTURE-EXTENSIONS.md](./FUTURE-EXTENSIONS.md).

---

## 9. Fallback Procedures

The following is an abbreviated summary. The complete Fallback Waterfall is in Governance Framework §7, which governs in the event of conflict.

| Trigger | Response |
|---------|----------|
| Vast.ai API unavailable < 2 Business Days | Use prior-day snapshot with disclosure |
| Vast.ai API unavailable 2–5 Business Days | Publish with Low Confidence flag, note API unavailability |
| Vast.ai API unavailable > 5 Business Days | Activate Fallback Level 2: secondary marketplace data (RunPod or equivalent) |
| Observation count < 5 for > 10 consecutive days | Initiate data source review; escalate to Oversight |
| Manifest Error identified | Correct via Restatement (Governance §8.3); publish correction notice |

---

## 10. Known Limitations

The following limitations are disclosed as part of CCIR's commitment to honest precision disclosure. Acknowledging limitations is not a weakness — it is what makes the index contract-safe.

### 10.1 Listed Prices vs. Executed Prices
CRI-H100 measures listed rental rates, not executed transaction prices. A listing at $1.80/GPU-hour may or may not be rented at that price. The index reflects willingness to supply, not confirmed transactions. This is a structural limitation of any open-data index — executed transaction data requires proprietary relationships incompatible with the open-methodology requirement. Ornn's OCPI, which uses executable offer data, provides a partial solution but at the cost of reproducibility.

### 10.2 Single Data Source
CRI-H100 v1.1.1 is based solely on Vast.ai marketplace data. Enterprise contracts, hyperscaler pricing, and other marketplace rates are not reflected. Vast.ai is a reproducible proxy for on-demand spot pricing but does not capture the full market.

### 10.3 Thin Observation Counts
Early-period daily observation counts of 8–20 Qualifying Listings are substantially below the data depth of established commodity or financial benchmarks. The Low Confidence flag and bootstrap confidence intervals are the mechanism for disclosing this limitation. CCIR publishes in all periods rather than suppressing data.

### 10.4 Intra-Model Performance Variance
Research published at GPGPU '26 (Silicon Data, March 2026) documents intra-model GPU performance variation of up to 38% within the H100 SXM model. CRI-H100 does not adjust for performance variance. Two listings at identical $/GPU-hour may represent economically different compute products. A performance-adjusted variant (CRI-H100-PA) is on the roadmap but requires a reproducible benchmark methodology not yet available.

### 10.5 Staleness of Listed Prices
Some listed prices may reflect outdated market conditions if providers have not actively updated their listings. The 7-day staleness filter mitigates this but does not eliminate it entirely.

### 10.6 No Volume Weighting
CRI-H100 uses the unweighted median. Volume data (number of GPUs per listing) is available but volume-weighting is not applied because large-listing volume may reflect inventory concentration rather than market-clearing prices. The median is more robust for credit reference purposes.

### 10.7 Geographic Approximation
The `geolocation` field in the Vast.ai API reflects the provider's registered data center location, not necessarily the physical location of the hardware. Edge cases may exist where hardware is misclassified geographically.

---

## 11. Governance Cross-References

This methodology document is part of a complete governance package. The following sections of the Governance Framework are directly relevant to specific methodology provisions:

| Methodology Topic | Governance Section |
|------------------|--------------------|
| Administrator and Calculation Agent roles | §2 |
| Data Input Design (no-submissions architecture) | §2.4 |
| Vast.ai as Third Party Data Provider | §9 |
| Fallback Waterfall (4 levels) | §7 |
| Benchmark Replacement Waterfall | §7.4 |
| Change Control (semantic versioning, notice periods) | §5 |
| Manifest Error and Restatement | §8 |
| Administrative Discretion (rules-based limits) | §8 |
| Annual Benchmark Review | §4.3 |
| Independent Audit commitment | §11 |
| Complaints Procedure | §12 |
| Rating Agency Disclosure | §15 |
| IOSCO Principles Alignment | Appendix C |

In the event of conflict between this methodology document and the Governance Framework, the Governance Framework governs with respect to process, and the Methodology governs with respect to calculation.

---

## 12. Independent Reproducibility

Any party can independently reproduce any published CRI-H100 value. The full process is documented in [REPRODUCIBILITY.md](./REPRODUCIBILITY.md).

**Quick start:**
```bash
git clone https://github.com/ccir-index/cri-h100
cd cri-h100
pip install -r requirements.txt
python pipeline/verify.py --end-date YYYY-MM-DD
```

The verification script compares your independently computed value against the published value and checks SHA-256 hashes for all raw data files in the calculation window.

---

## 13. Multi-Model Collection Context

CCIR collects data for 9 GPU models daily. Three models are currently published as live indices: H100 SXM (CRI-H100), A100 SXM (CRI-A100), and L40S (CRI-L40S). Remaining models are archived to support future depreciation curve analysis (CRI-D, Stage 2).

| Collection ID | GPU Model | Purpose |
|--------------|-----------|---------|
| h100-sxm-us | H100 SXM | **CRI-H100 — Live** |
| a100-sxm-us | A100 SXM | **CRI-A100 — Live** |
| l40s-us | L40S | **CRI-L40S — Live** |
| a100-sxm-us | A100 SXM | Depreciation curve data |
| a100-pcie-us | A100 PCIe | A100 variant |
| h200-us | H200 | Next-generation transition data |
| h200-nvl-us | H200 NVL | H200 variant |
| h100-pcie-us | H100 PCIe | H100 variant |
| v100-us | Tesla V100 | Long-horizon depreciation |
| l40s-us | L40S | Inference-class data |
| rtx4090-us | RTX 4090 | Consumer market context |

---

*This document is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Any party may freely cite, reference, or reproduce this methodology with attribution.*

*© 2026 Compute Credit Index Research LLC*
