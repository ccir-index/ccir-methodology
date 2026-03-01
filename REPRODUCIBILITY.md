# CRI-H100 Independent Verification Guide

Any party can independently reproduce any published CRI-H100 value from the raw data. This document provides the complete step-by-step process.

---

## What You Can Verify

For any published CRI-H100 value, you can independently confirm:

1. **The raw data** — the exact listings observed on each collection day
2. **The SHA-256 hash** — that the raw data files have not been altered since collection
3. **The calculation** — that the published value is the correct output of the methodology applied to the raw data
4. **The audit trail** — the full calculation detail including observation counts, outlier removals, and dispersion metrics

---

## Prerequisites

```bash
git clone https://github.com/ccir-index/cri-h100
cd cri-h100
pip install -r requirements.txt
```

Requirements: Python 3.9+, pandas, numpy, requests

---

## Step 1 — Download the Raw Data

All raw data is in the `data/raw/` directory of the cri-h100 repository. Each collection day produces two files:

```
data/raw/YYYY-MM-DD.csv           # Daily filtered snapshot (qualifying listings)
data/raw/YYYY-MM-DD.meta.json     # Collection metadata including SHA-256 hash
```

The CSV contains one row per qualifying listing with columns:
- `date` — collection date
- `gpu_name` — GPU model
- `geolocation` — provider location
- `reliability2` — provider reliability score
- `num_gpus` — number of GPUs in listing
- `dph_total` — total $/hour for the listing
- `price_per_gpu` — computed $/GPU-hour (dph_total / num_gpus)
- `start_date` — listing creation date

---

## Step 2 — Verify the SHA-256 Hash

Each `.meta.json` file contains a SHA-256 hash of the corresponding `.csv` file, recorded at the time of collection and committed to the public repository.

To verify a specific day's data has not been altered:

```bash
python pipeline/verify.py --check-hash --date 2026-03-05
```

Expected output:
```
Verifying SHA-256 for 2026-03-05...
  Stored hash:   a3f2b1c9d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1
  Computed hash: a3f2b1c9d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1
  MATCH ✓  Data integrity confirmed for 2026-03-05
```

A hash mismatch indicates the raw data file has been altered after collection — this should be reported immediately to research@ccir.io.

---

## Step 3 — Reproduce the Published Index Value

To reproduce a specific weekly CRI-H100 publication:

```bash
python pipeline/verify.py --end-date YYYY-MM-DD
```

where `YYYY-MM-DD` is the Wednesday ending the calculation window (the day before Thursday publication).

Example:
```bash
python pipeline/verify.py --end-date 2026-03-04
```

Expected output:
```
Reproducing CRI-H100 — week ending 2026-03-04
------------------------------------------------------------
  Collection window: 2026-02-27 to 2026-03-04

  Daily breakdown:
    2026-02-27:  13 qualifying obs → 12 after outlier removal, median $1.8650
    2026-02-28:  14 qualifying obs → 13 after outlier removal, median $1.8420
    2026-03-01:  11 qualifying obs → 11 after outlier removal, median $1.8510
    2026-03-02:  12 qualifying obs → 12 after outlier removal, median $1.8440
    2026-03-03:  15 qualifying obs → 14 after outlier removal, median $1.8380
    2026-03-04:  13 qualifying obs → 12 after outlier removal, median $1.8350

  Total pooled observations: 74
  Weekly median (pooled):    $1.8440
  95% confidence interval:   [$1.8210, $1.8680]
  Low Confidence flag:        No

  Reproduced value:  $1.8440
  Published value:   $1.8440
  MATCH ✓  CRI-H100 = $1.8440 independently verified.
```

---

## Step 4 — Verify the Calculation Steps Manually

The calculation follows this deterministic sequence for each day in the window:

```
for each day in trailing_7_days:
    load daily snapshot (data/raw/YYYY-MM-DD.csv)

    FILTER — hardware:
        keep rows where gpu_name == "H100 SXM"

    FILTER — geography:
        keep rows where geolocation ends with ", US"

    FILTER — reliability:
        keep rows where reliability2 >= 0.90

    FILTER — availability:
        keep rows where rentable == true AND rented == false

    FILTER — minimum GPUs:
        keep rows where num_gpus >= 1

    FILTER — staleness:
        keep rows where start_date >= (collection_date - 7 days)

    compute: price_per_gpu = dph_total / num_gpus

    OUTLIER REMOVAL:
        trimmed_mean = mean of middle 80% of price_per_gpu values
        stdev = standard deviation of all price_per_gpu values
        exclude rows where |price_per_gpu - trimmed_mean| > 2.5 * stdev

    if qualifying_count >= 10:
        mark day as VALID
        daily_median = median(price_per_gpu of remaining rows)
    else:
        mark day as EXCLUDED (insufficient observations)

if valid_days >= 3:
    weekly_median = median(all price_per_gpu values from valid days, pooled)
    publish CRI-H100 = weekly_median
    if valid_days < 7 OR any_day_count < 10:
        apply Low Confidence flag
else:
    publish with Low Confidence flag
    weekly_median = median(all available observations regardless of daily threshold)
```

---

## Step 5 — Interpret Any Discrepancies

Minor discrepancies (< 0.001%) may occur due to:

- **API timing**: CCIR collects at approximately 9:00 AM Central. A third party querying the API at a different time may observe a slightly different listing set. The SHA-256 hash records the exact snapshot CCIR used.
- **Floating point**: Different Python environments may produce marginally different floating point results. The published value is always rounded to 4 decimal places.

Discrepancies > 0.01% should be reported to research@ccir.io with the specific date and reproduced value. CCIR commits to investigating and responding within 5 Business Days.

---

## Step 6 — Verify the Published Audit File

Each weekly calculation produces a full audit file at:

```
outputs/audits/cri-h100-YYYY-MM-DD.audit.json
```

This file contains the complete calculation trail: every daily observation, the outlier removal process, the pooled observation set, and the final median. It is committed to the public repository at publication time and can be cross-referenced against your independent reproduction.

---

## Reporting Verification Results

If you successfully reproduce a CRI-H100 value, no action is required. If you identify a discrepancy, please report it to [research@ccir.io](mailto:research@ccir.io) with:

- The publication date in question
- Your reproduced value
- The published value
- Your Python version and operating system
- Any error messages produced by verify.py

CCIR will investigate and respond within 5 Business Days under the Complaints Procedure (Governance Framework §12).

---

© 2026 Compute Credit Index Research LLC · CC BY 4.0
