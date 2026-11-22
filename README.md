# Comparative effectiveness analysis of the treatment of asymptomatic and symptomatic complex aortic aneurysms

Biostatistics capstone project supervised by Dr. Sarah Zettervall.

Team members:

- Jennifer Ci
- Thu Vu
- Lily Hanyi Wang

## Publication

Ghosh, R., Dansey, K. D., **Wang, H.**, Ci, X., Vu, T., O'Donnell, T. F. X., Wang, G. J., Schermerhorn, M. L., & Zettervall, S. L. (2024). *Symptomatic presentation for complex aortic aneurysms is associated with increased mortality when compared with asymptomatic patients.* Society for Clinical Vascular Surgery (SCVS) Annual Symposium, 2024.

## Project summary

**Question.** Among patients undergoing fenestrated/branched endovascular repair (FBEVAR) of a complex aortic aneurysm, is a symptomatic (non-ruptured) presentation associated with worse survival and perioperative outcomes than an asymptomatic presentation?

**Data.** Procedure-level (PROC) and long-term follow-up (LTF) files from the Society for Vascular Surgery Vascular Quality Initiative (SVS VQI) TEVAR & Complex EVAR registry. The VQI data are not included in this repository.

Analytic cohort (`preprocessing.Rmd`):

- Excluded ruptured aneurysms; kept elective, urgent and emergent cases.
- Kept aneurysm and dissection pathologies. Excluded trauma, thrombus, fistula and other pathologies.
- Kept proximal disease in zones 2 to 9, with the distal zone beyond the proximal zone.
- Kept FBEVAR procedures with at least one treated renal, SMA or celiac branch.
- Final cohort: **3,757 patients** (3,315 asymptomatic, 442 symptomatic).

**Methods.**

| Step | File | What it does |
|---|---|---|
| Data cleaning | `preprocessing.Rmd` | Merges registry extracts, applies the exclusion criteria, recodes demographics, comorbidities, operative and outcome variables, and derives eGFR (CKD-EPI 2021) and branch-treatment variables |
| Descriptive statistics | `univariate_analysis.Rmd` | Table 1 comparisons by presentation (Welch t-test / chi-square), plus center and physician case volumes |
| Survival analysis | `survival_analysis.Rmd` | Kaplan-Meier curves for all-cause mortality and time to first reintervention. Unadjusted Cox model, and an adjusted Cox model stratified by sex with robust SEs clustered on center |
| Secondary outcomes | `GEE.Rmd` | Logistic GEE models (clustered on center, independence working correlation) for 13 perioperative outcomes: length of stay, ICU stay, ≥50% GFR decline, stroke, spinal cord ischemia, dialysis, respiratory and other complications, reintervention, and more |
| Long-term follow-up | `long term.R` | Combines LTF extracts and derives reintervention events |
| Figures | `forestplots.R` | Forest plots of unadjusted and adjusted odds ratios from the GEE models |

The adjusted models include age group, sex (as a stratum in the Cox model), smoking, prior aortic surgery, CHF, preoperative dialysis, pathology and aneurysm extent. In the GEE models, extent is binary: juxtarenal or type IV TAAA vs. type I, II, III or V TAAA. The GFR model swaps preoperative dialysis for preoperative GFR category and number of treated renal arteries.

**Key results** (from the knitted reports in this repository):

- **All-cause mortality:** symptomatic presentation was associated with higher mortality. The unadjusted HR was 2.06 (95% CI 1.65 to 2.58), and the adjusted HR was **1.84 (95% CI 1.41 to 2.41)**, p < 0.001.
- **Perioperative outcomes (adjusted GEE odds ratios):** symptomatic patients had higher odds of total length of stay over 7 days (OR 3.79, 95% CI 2.99 to 4.81), ICU stay over 4 days (OR 2.08), postoperative LOS over 7 days (OR 2.46), respiratory complications (OR 2.17), any complication (OR 1.44), postoperative dialysis (OR 1.78) and reintervention (OR 1.48).
- The associations with stroke, spinal cord ischemia, ≥50% GFR decline, leg ischemia and intestinal ischemia were not statistically significant after adjustment.

## Repository contents

- `*.Rmd`: analysis notebooks. Each has a knitted `*.pdf` report of the same name.
- `*.R`: standalone scripts for long-term follow-up processing and forest plots.

To reproduce, place the registry extracts in a local `data/` folder (see the paths in `preprocessing.Rmd`) and knit the notebooks in order: preprocessing, univariate analysis, survival analysis, GEE. Patient-level data must not be committed to this repository.
