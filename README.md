# CCIR Methodology

**Compute Credit Index Research LLC — Methodology & Governance Repository**

This repository contains the complete methodology, governance framework, and IOSCO disclosure package for the CCIR family of GPU compute reference indices. All documents are published under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

---

## The Indices

CCIR publishes five daily GPU compute reference rates and a companion rate card series, all governed under this methodology.

| Index | Model | Primary Use |
|-------|-------|-------------|
| **CRI-B200** | NVIDIA B200 SXM 192GB | Frontier training; next-gen displacement signal |
| **CRI-H200** | NVIDIA H200 SXM5 141GB | Large-model training; memory-bound inference |
| **CRI-H100** | NVIDIA H100 SXM5 80GB | Current training tier; **primary collateral reference rate** |
| **CRI-A100** | NVIDIA A100 SXM4 80GB | Prior-gen inference and batch; cascade velocity signal |
| **CRI-L40S** | NVIDIA L40S 48GB | Inference-optimized; commodity cascade pricing |
| **CRI-R** | Hyperscaler + RunPod | Enterprise rate card context (AWS, Azure, GCP, RunPod) |

Published daily. Full history at [github.com/ccir-index/ccir-data](https://github.com/ccir-index/ccir-data). Live indices at [ccir.io](https://ccir.io).

---

## What These Indices Measure

Each CRI index is the **trailing 7-day median $/GPU-hour** for on-demand spot rental listings in the US marketplace, calculated using a **median-of-medians architecture**: a per-source median is computed independently for each contributing data source, and the published index value is the unweighted median across those per-source medians. Each source casts one vote regardless of listing count.

The no-submissions design means no market participant can influence the index by submitting rates. All data is sourced exclusively from public APIs.

---

## Documents in This Repository

| Document | Description |
|----------|-------------|
| [`METHODOLOGY-v1.0.0.md`](./METHODOLOGY-v1.0.0.md) | Complete calculation methodology — data sources, quality filters, normalization, median-of-medians calculation, confidence flags, fallback provisions, change control |
| [`GOVERNANCE-v1.0.0.md`](./GOVERNANCE-v1.0.0.md) | Governance framework — Administrator role, Calculation Agent, Oversight Board, change control policy, complaints procedure, IOSCO alignment, rating agency disclosure |
| [`IOSCO-Governance-Package-v1.0.pdf`](./IOSCO-Governance-Package-v1.0.pdf) | External disclosure document — full pipeline architecture, source registry, 19-principle IOSCO mapping with implementation evidence, Bronze/Silver/Gold lineage schema, pipeline controls (C-01–C-23). Intended for rating agencies and legal counsel. |
| [`CHANGELOG.md`](./CHANGELOG.md) | Version history for all methodology and governance changes |

---

## Calculation Summary

```
Data sources:  Vast.ai · Shadeform · RunPod  (equal weight — one vote per source)
Window:        Trailing 7 days
Filter:        Target GPU model · US geography · verified_host · reliability ≥ 90% · datacenter_flag
Outlier:       IQR × 3 per source group (Silver layer)
Calculation:   Per-source median → median-of-medians
Confidence:    high  = n_sources ≥ 3 AND n_valid_days ≥ 5
               medium = n_sources ≥ 2 AND n_valid_days ≥ 3
               low   = otherwise
               (valid day = calendar day with 2+ distinct sources contributing)
Publication:   Daily by 17:00 UTC · CSV · github.com/ccir-index/ccir-data
```

---

## Confidence Flags

Every published value carries a confidence flag. The flag reflects both source breadth and observation density — thin source coverage and thin observation windows each independently trigger a downgrade.

| Flag | Criteria | Credit Agreement Use |
|------|----------|----------------------|
| `high` | ≥ 3 sources AND ≥ 5 valid days | Standard trigger use |
| `medium` | ≥ 2 sources AND ≥ 3 valid days | Trigger use with buffer; see covenant reference |
| `low` | Otherwise | Informational only; not appropriate for standalone credit reference |

---

## Reference Language for Credit Agreements

```
"The Compute Reference Rate means the [CRI-H100] index value as published by
Compute Credit Index Research LLC (ccir.io) for the most recent date on which
a high or medium confidence value is available, calculated in accordance with
CCIR Methodology v1.0.0 or any successor version adopted in accordance with
the CCIR Change Control Policy."
```

Practitioner reference documents — including covenant structures, fallback waterfall language, and confidence regime applicability — are available at [ccir.io](https://ccir.io).

---

## Change Control

- **Material changes** (calculation method, source additions/removals, confidence thresholds): 60 days' public notice before effective date.
- **Non-material changes** (documentation clarifications, governance updates): 14 days' notice.
- All changes are recorded in [`CHANGELOG.md`](./CHANGELOG.md) with semantic version bump and classification.

The `methodology_version` field in every published CSV row identifies the exact version under which that value was calculated, enabling version-conditional interpretation of any historical data point.

---

## IOSCO Alignment

CCIR indices are governed with reference to the [IOSCO Principles for Financial Benchmarks (July 2013)](https://www.iosco.org/library/pubdocs/pdf/IOSCOPD415.pdf). The full 19-principle mapping with pipeline implementation evidence is in [`IOSCO-Governance-Package-v1.0.pdf`](./IOSCO-Governance-Package-v1.0.pdf).

Key structural features relevant to IOSCO compliance:

- **No submissions** — all data from public APIs; `collection_method` field records `api_rest` or `api_graphql` on every Bronze row
- **Immutable audit trail** — Delta Lake time-travel preserves full Bronze/Silver/Gold state; `raw_payload` stores complete API response per listing
- **Deterministic calculation** — no discretionary overrides; median-of-medians is fully rules-based
- **Published methodology** — independently reproducible by any third party from public data

---

## Independent Reproducibility

Any party can independently reproduce any published index value. See [`REPRODUCIBILITY.md`](./REPRODUCIBILITY.md) for the full verification procedure.

The `raw_payload` field in the Bronze Delta table stores the complete raw API response for every collected listing. The Bronze → Silver → Gold lineage chain is maintained via `row_id` → `bronze_row_id` foreign keys. GitHub commit history provides an immutable external audit record with observation date in every commit message.

---

## Contact

**Compute Credit Index Research LLC**
research@ccir.io · [ccir.io](https://ccir.io)

For pre-transaction audit coordination, rating agency disclosure packages, or legal counsel inquiries, contact research@ccir.io.

---

*Published under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Any party may freely cite, reproduce, or reference these documents with attribution to Compute Credit Index Research LLC.*
