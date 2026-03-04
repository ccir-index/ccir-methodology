# CCIR Methodology Changelog

All changes to CCIR methodology and governance documents are recorded here. This log follows the versioning policy defined in `VERSIONING.md`.

---

## Methodology

### v1.1.0 — February 2026
**Type:** Initial publication (no prior version)
**Material change:** No (initial release)
**Effective date:** February 2026
**Approved by:** CCIR Administrator

**Changes:**
- Initial publication of CRI-H100 methodology
- Multi-model collection framework (9 GPU models)
- US-only geographic scope
- Quality filters: reliability ≥ 0.90, active within 7 days, unrented, ≥ 1 GPU
- Outlier removal: trimmed mean ± 2.5σ
- Calculation: trailing 7-day median $/GPU-hour
- Low Confidence flag: < 10 daily observations or < 3 valid days
- Weekly Thursday publication cadence
- SHA-256 tamper-evident audit trail

**Compared to v1.0.0 (internal pre-publication draft):**
- Added multi-model collection (v1.0.0 was H100 SXM only)
- Formalized Low Confidence flag thresholds
- Added per-model directory structure to data archive

---

### v1.1.1 — March 2026
**Type:** Non-material addition
**Material change:** No
**Effective date:** March 2026
**Approved by:** CCIR Administrator

**Changes:**
- Added `collect_reference_rates.py` pipeline collecting posted rate cards from Azure, AWS, GCP, and RunPod
- CRI-R companion series launched: weekly snapshots of H100, A100, H200, and V100 posted rates across 4 platforms (18 rate points per collection)
- Output published at `data/reference-rates/YYYY-MM-DD.json/.csv/.meta.json` with SHA-256 provenance
- Azure and AWS sources require no authentication (fully reproducible); GCP and RunPod require a freely obtainable API key (disclosed in metadata)
- CRI-R is **not** an IOSCO-compliant benchmark and is **not** an input to CRI-H100. It is contextual reference data published alongside CRI-H100 to enable reliability premium spread analysis
- Adding any CRI-R source as an official CRI-H100 benchmark input would constitute a Material Methodology Change under Governance §5.2 (60-day advance notice required)

---

### v1.2.0 — Planned
**Type:** Non-material enhancement
**Planned changes:**
- Add CRI-A100 as a formally published index (data collection already underway)
- Add cross-model comparison table to weekly publication

---

### v2.0.0 — Planned (contingent on broker data partnership)
**Type:** Material change
**Planned changes:**
- Add CRI-S (rental-resale spread index) using broker-contributed resale data
- Multi-source data input framework
- New aggregation methodology for contributed data

---

## Governance

### v1.0 — February 2026
**Type:** Initial publication
**Material change:** No (initial release)
**Effective date:** February 2026

**Highlights:**
- IOSCO Principles for Financial Benchmarks alignment (all 19 principles mapped, Appendix C)
- No-submissions architecture (eliminates LIBOR-style submitter conflicts)
- Administrator/Calculation Agent separation
- 4-level fallback waterfall
- 4-step benchmark replacement waterfall
- Formalized Annual Benchmark Review
- Semantic versioning with 60-day notice for material changes
- 5-year data retention commitment
- Annual independent governance audit commitment
- Complaints procedure (5 Business Day acknowledgment, 30 Business Day response)

---

*Changes are recorded in this file at the time of publication. For pending changes under consultation, see the Issues tab of this repository.*
