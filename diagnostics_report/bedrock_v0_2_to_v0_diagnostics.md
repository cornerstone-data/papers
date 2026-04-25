## 3. Diagnostic results: v0.2 vs USEEIO and CEDA-US benchmarks

v0.2 is compared against two external benchmarks in parallel:
- **USEEIO** — USEEIOv2.6.0-phoebe-23 on [Zenodo](https://zenodo.org/records/17457336).
- **CEDA-US** — US portion of the multi-regional CEDA model.

### 3.1. Headline numbers
| Metric | vs USEEIO | vs CEDA-US |
|---|---|---|
| Median EFs percentage change | *TBD* | *TBD* |
| Median EFs absolute change | *TBD* | *TBD* |
| Number of sectors shifting > *20%* in EFs | *TBD* | *TBD* |
| Top 3 flags by impact on B-matrix entries | *TBD* | *TBD* |

### 3.2. Figures (USEEIO and CEDA-US benchmarks, side-by-side)

#### (1) Histograms

**(1a) EF %-change distribution** — 2×2 panels of numerator / denominator percent-diff distributions.

| vs USEEIO | vs CEDA-US |
|---|---|
| ![EF %-change vs USEEIO](bedrock/utils/validation/analysis/output/useeio/ef_n_perc_diff_histogram.png) | ![EF %-change vs CEDA-US](bedrock/utils/validation/analysis/output/ceda/ef_n_perc_diff_histogram.png) |

**(1b) EF absolute change distribution**

| vs USEEIO | vs CEDA-US |
|---|---|
| ![EF absolute change vs USEEIO](bedrock/utils/validation/analysis/output/useeio/ef_abs_change_histogram.png) | ![EF absolute change vs CEDA-US](bedrock/utils/validation/analysis/output/ceda/ef_abs_change_histogram.png) |

#### (2) x–y plots

**(2a) EF size (x-axis) vs EF %-change (y-axis)**

| vs USEEIO | vs CEDA-US |
|---|---|
| ![EF size vs %-change, USEEIO](bedrock/utils/validation/analysis/output/useeio/ef_pct_change_vs_ef_size.png) | ![EF size vs %-change, CEDA-US](bedrock/utils/validation/analysis/output/ceda/ef_pct_change_vs_ef_size.png) |

**(2b) EF absolute change (x-axis) vs EF %-change (y-axis)**

| vs USEEIO | vs CEDA-US |
|---|---|
| ![EF abs vs %-change, USEEIO](bedrock/utils/validation/analysis/output/useeio/ef_pct_change_vs_abs_change.png) | ![EF abs vs %-change, CEDA-US](bedrock/utils/validation/analysis/output/ceda/ef_pct_change_vs_abs_change.png) |

#### (3) Stacked-bar chart — sector-level net-change attribution

| vs USEEIO | vs CEDA-US |
|---|---|
| ![BLy sector stacked net change, USEEIO](bedrock/utils/validation/analysis/output/useeio/bly_sector_stacked_net_change.png) | ![BLy sector stacked net change, CEDA-US](bedrock/utils/validation/analysis/output/ceda/bly_sector_stacked_net_change.png) |

