# CCIR Reproducibility Guide

**Applies to:** CRI-H100, CRI-A100, CRI-L40S, CRI-H200, CRI-B200
**Current methodology version:** v1.0.0 (March 2026)

Any party can independently verify any published CCIR index value using publicly available data. This document explains how.

---

## What "Reproducible" Means Here

A published CRI-H100 value is reproducible if an independent party can:

1. **Collect** the same raw listings from the same sources using the same filters
2. **Apply** the same normalization and outlier removal procedures
3. **Compute** the same statistic over the same trailing window
4. **Arrive at** the same index value (or a value within rounding tolerance)

All three sources contribute equally to the published index value under the median-of-medians architecture — no single source is weighted more heavily than another. Vast.ai requires no authentication; Shadeform and RunPod require free API key registration. An independent verifier can substantially approximate any index value using Vast.ai alone given its no-authentication access, but the published value reflects all three sources equally.

---

## Data Sources

| Source | Authentication | GPU models collected | GPU models collected |
|--------|------|---------------|---------------------|
| [Vast.ai](https://vast.ai) | None — public REST API | H100 SXM/PCIe, A100 SXM4/PCIe, L40S, H200 SXM/NVL, B200 SXM |
| [Shadeform](https://shadeform.ai) | Free API key | H100, H200, A100, L40S, B200 (30+ provider pool) |
| [RunPod](https://runpod.io) | Free API key | H100, H200, A100, L40S, B200 |

---

## Step-by-Step Verification

### Step 1 — Determine the observation window

For any observation date `D`, the calculation window is:

```
window_start = D − 7 calendar days (UTC)
window_end   = D (UTC, end of day)
```

All qualifying listings with `collected_at` between `window_start` and `window_end` (inclusive) are eligible.

### Step 2 — Collect raw listings from Vast.ai

Query the Vast.ai bundles endpoint for each GPU model of interest:

```
GET https://console.vast.ai/api/v0/bundles/
  ?q={"gpu_name": {"eq": "<GPU_NAME>"}, "rentable": {"eq": true}}
```

GPU name strings by index:

| Index | GPU name string |
|-------|----------------|
| CRI-H100 | `H100 SXM` |
| CRI-A100 | `A100 SXM4` |
| CRI-L40S | `L40S` |
| CRI-H200 | `H200` |
| CRI-B200 | `B200` |

Derive per-GPU-hour price:

```python
price_per_gpu_hr = offer["dph_total"] / max(1, offer["num_gpus"])
```

### Step 3 — Apply quality filters

A listing must pass **all** filters to become a qualifying observation:

| Filter | Criterion |
|--------|-----------|
| GPU model | Matches target hardware filter (via MODEL_LOOKUP below) |
| Rentable | `rentable == true` |
| Not rented | `rented == false` |
| Reliability | `reliability2 >= 0.90` (Vast.ai); equivalent for other sources |
| Min GPU count | `num_gpus >= 1` |
| Staleness | Listed within 7 calendar days of collection date |
| Geography | US data center |

**MODEL_LOOKUP key precedence** (first match wins):

```
"H100 SXM"  → NVIDIA H100 SXM5 80GB / SXM
"H100 PCIE" → NVIDIA H100 PCIe 80GB / PCIe
"H100"      → NVIDIA H100 SXM5 80GB / SXM
"A100 SXM4" → NVIDIA A100 SXM4 80GB / SXM
"A100 SXM"  → NVIDIA A100 SXM4 80GB / SXM
"A100 PCIE" → NVIDIA A100 PCIe 80GB / PCIe
"A100"      → NVIDIA A100 SXM4 80GB / SXM
"L40S"      → NVIDIA L40S 48GB / PCIe
"H200 SXM"  → NVIDIA H200 SXM5 141GB / SXM
"H200 NVL"  → NVIDIA H200 NVL 141GB / NVLink
"H200"      → NVIDIA H200 SXM5 141GB / SXM
"B200 SXM"  → NVIDIA B200 SXM 192GB / SXM
"B200"      → NVIDIA B200 SXM 192GB / SXM
```

### Step 4 — Apply outlier removal

Applied per `(source_id, gpu_model_canonical)` group:

```python
Q1 = percentile(prices, 0.25)
Q3 = percentile(prices, 0.75)
IQR = Q3 - Q1
lower_fence = Q1 - 3 * IQR
upper_fence = Q3 + 3 * IQR

is_outlier = (price < lower_fence) OR (price > upper_fence)
```

Remove all rows where `is_outlier == True`. Retain outlier rows for audit purposes.

### Step 5 — Count valid days and assign confidence flag

A **Valid Day** is any calendar day on which at least **2 distinct source IDs** contributed qualifying observations. Single-source days do not count.

```python
# Per calendar day: count distinct source_ids contributing
daily_source_counts = {
    day: count_distinct(source_id)
    for day, listings in group_by_date(qualifying_observations)
}
n_valid_days = sum(1 for count in daily_source_counts.values() if count >= 2)
source_count  = count_distinct(source_id for obs in qualifying_observations)

if source_count >= 3 and n_valid_days >= 5:   confidence_flag = "high"
elif source_count >= 2 and n_valid_days >= 3:  confidence_flag = "medium"
else:                                           confidence_flag = "low"
```

If total qualifying observations is zero: `cri_value = None`, `confidence_flag = "low"`.

### Step 6 — Compute the index value

**Median-of-medians architecture** — each source casts one vote regardless of listing count:

```python
# Step 1: per-source median
source_medians = {
    source_id: median(price_per_gpu_hr for obs in group)
    for source_id, group in group_by_source(qualifying_observations)
}

# Step 2: median across per-source medians
cri_value = median(source_medians.values())
```

Also record: `obs_p25` / `obs_p75` / `obs_min` / `obs_max` as the 25th/75th percentile, min, and max of the per-source medians. Record `total_observations`, `n_valid_days`, `source_count`.

### Step 7 — Compare against published value

Published values are in [`ccir-index/ccir-data`](https://github.com/ccir-index/ccir-data):

```
cri-h100/CRI-H100-history.csv
cri-a100/CRI-A100-history.csv
cri-l40s/CRI-L40S-history.csv
cri-h200/CRI-H200-history.csv
cri-b200/CRI-B200-history.csv
```

Find the row where `observation_date == D`. Expected agreement is within floating-point rounding tolerance (< 0.0001 $/GPU-hour). The `methodology_version` column confirms the calculation rules in effect for that publication.

---

## Multi-Source Verification (Shadeform + RunPod)

**Shadeform:** `GET https://api.shadeform.ai/v1/instances/types` with `X-API-KEY: <your_key>`. Filter for target GPU type. Derive `price_per_gpu = hourly_price / 100 / num_gpus` (prices are in cents).

**RunPod:** GraphQL query at `https://api.runpod.io/graphql` with `Authorization: Bearer <your_key>`. Use `minimumBidPrice` as the spot price.

**Region normalization note:** RunPod does not expose geographic metadata. The production pipeline maps `region_raw = "runpod_global"` to `region_normalized = "us"`. Independent verifiers should treat all RunPod listings as US-region. This mapping is reviewed quarterly per Governance Framework §9.3.

---

## Expected Sources of Minor Variance

| Source | Explanation | Magnitude |
|--------|------------|-----------|
| Collection timing | Independent collection at a different time yields a slightly different listing set | 0–5% of listings |
| Approximate quantile | Spark's `percentile_approx` agrees with exact median within rounding; applied per-source then across source medians | < 0.0001 $/GPU-hr |
| Availability snapshot | Listings appearing/disappearing between production and verification runs | Small impact on median |

Differences exceeding 5% of the published value that cannot be explained by the above should be reported to research@ccir.io as a potential Manifest Error.

---

## Audit Trail Access

The production pipeline maintains a complete Bronze-to-Gold lineage chain:

- **Bronze:** Every raw listing stored with UUID `row_id`, `source_id`, `collected_at`, `raw_payload`
- **Silver:** Every normalized row includes `bronze_row_id`, `is_outlier` flag, `gpu_model_canonical`
- **Gold:** Every published value includes `window_start`, `window_end`, `computed_at`, `methodology_version`, `confidence_flag`, `total_observations`, `n_valid_days`
- **GitHub:** Every export creates a dated commit to `ccir-index/ccir-data` — immutable external audit record

For pre-transaction audit services or raw data archive access: research@ccir.io. Response commitment: 5 Business Days.

---

*For the full calculation specification, see [`METHODOLOGY-v1.0.0.md`](./METHODOLOGY-v1.0.0.md).*
*For governance provisions including audit rights and complaints, see [`GOVERNANCE-v1.0.0.md`](./GOVERNANCE-v1.0.0.md).*

*© 2026 Compute Credit Index Research LLC. Published under CC BY 4.0.*
