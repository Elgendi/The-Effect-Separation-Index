# Effect Separation Index

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Elgendi/The-Effect-Separation-Index/blob/main/ESI_complete_Colab_analysis.ipynb)
[![Python 3.12](https://img.shields.io/badge/Python-3.12-blue.svg)](https://www.python.org/)
![Reproducible analysis](https://img.shields.io/badge/analysis-reproducible-brightgreen.svg)
![Status](https://img.shields.io/badge/status-manuscript%20revision-orange.svg)

This repository contains the complete reproducible analysis for:

> Elgendi, M. *The Effect Separation Index: A Composite Diagnostic for Evaluating Group Separation Across Measurement Scales* Communications AI & Computing.

The repository is intentionally minimal. The single notebook downloads the public datasets, executes the simulations and real-data analyses, generates every main and supplementary figure, creates the manuscript tables and source-data workbook, validates the required outputs, and packages the results.

## Files

- `ESI_complete_Colab_analysis.ipynb` — self-contained Google Colab analysis.
- `README.md` — documentation and execution instructions.

No local data upload is required. The notebook retrieves the public datasets used in the study and records their source URLs and SHA-256 hashes.

## Run in Google Colab

1. Click **Open in Colab** above.
2. In Colab, select **Runtime → Run all**.
3. Accept the browser download when the final cell creates `ESI_Colab_complete_outputs.zip`.

A standard CPU runtime is sufficient. The complete run may take approximately 10–20 minutes because the real-data analysis uses 2,000 within-group bootstrap replicates and the simulations use 500 paired Monte Carlo replicates per condition.

## Analysis specification

The notebook implements the final analysis reported in the manuscript:

- transformation parameters are estimated from pooled observations;
- the same fitted mapping is applied to both groups;
- every candidate transformation is evaluated on identical simulated samples;
- real-data raw and transformed results use identical bootstrap indices;
- the primary overlap estimate uses Gaussian kernel density estimation with a robust Silverman bandwidth;
- ESI uses the median absolute Cohen's \(d\), the standard deviation of the **signed** resampled \(d\), and mean distributional overlap;
- transformation effects are reported as
  \(\Delta\mathrm{ESI}=\mathrm{ESI}_{\mathrm{transformed}}-\mathrm{ESI}_{\mathrm{raw}}\);
- ESI is treated as a sample-dependent diagnostic, not an intrinsic population effect size or a universal transformation optimizer.

## Generated outputs

Running the notebook creates `ESI_Colab_complete_outputs.zip`, containing:

- Figures 1–3 in publication-quality PNG and PDF formats;
- Supplementary Figures S1–S4;
- complete figure-level source-data CSV files;
- the 43-variable real-data results;
- the eight-dataset inventory and direct source links;
- `Source_Data_Colab.xlsx`, with the journal source data;
- downloaded public datasets and provenance information;
- analysis metadata, random seed, package versions, and validation results.

## Effect Separation Index

For resampled effect estimates \(d^{(b)}\) and overlap estimates \(\mathrm{OVL}^{(b)}\),

\[
\mathrm{ESI}=
\frac{\operatorname{median}(|d^{(b)}|)}
{\operatorname{sd}(d^{(b)})}
\left[1-\operatorname{mean}(\mathrm{OVL}^{(b)})\right].
\]

The denominator is calculated from the signed effect estimates. ESI depends on sample size and the resampling and density-estimation specifications. It should be interpreted with Cohen's \(d\), overlap, uncertainty, graphical displays, and scientific context.

## Reproducibility

- Master random seed: `2025`
- Monte Carlo replicates per simulation condition: `500`
- Real-data bootstrap replicates: `2,000`
- KDE evaluation grid: `512` points
- Primary environment: Python 3.12

The notebook installs its Python dependencies in the first executable cell. For the version of record, use the notebook associated with the manuscript submission or archived software release.

## Data

All analyzed datasets are publicly available. The generated dataset manifest records the exact download location, analyzed groups and variables, sample counts, missing-data handling, and file hashes. The 918-record Heart Disease Prediction dataset and the 299-record Heart Failure Clinical Records dataset are treated as distinct datasets. No artificial median split is used.

## Scientific motivation

Very small (p)-values can coexist with substantial visual and distributional overlap, especially in large datasets. This occurs because statistical evidence, standardized effect magnitude, estimation precision, and distributional similarity describe related but different properties.

- A (p)-value evaluates the compatibility of the data with a statistical model under a null hypothesis and is strongly influenced by sample size.
- Cohen's (d) quantifies a standardized mean difference but does not quantify density overlap or the resampling stability of the estimated effect.
- The overlap coefficient measures shared distributional support but does not describe effect magnitude or estimation precision.
- ESI combines effect magnitude, stability, and overlap while retaining each component for transparent interpretation.

The purpose of ESI is therefore not to replace established statistics. Its role is to diagnose how these complementary properties coexist on a specified measurement scale and how they change when a common transformation is applied.

## Interpretation of the components

| ESI component | Statistical role | Direction associated with higher ESI |
|---|---|---|
| (operatorname{median}(|d^{(b)}|)) | Typical standardized effect magnitude | Larger |
| (operatorname{sd}(d^{(b)})) | Resampling variability of the signed effect estimate | Smaller |
| (1-operatorname{mean}(mathrm{OVL}^{(b)})) | Distributional separation | Larger |

The multiplicative form ensures that limited effect magnitude or extensive overlap attenuates the composite value. The inverse stability term increases ESI when the estimated effect becomes more reproducible across resamples.

Because estimation variability usually decreases as sample size increases, ESI is sample dependent. Two ESI values should not be compared without considering the sample size, resampling design, effect definition, density estimator, and bandwidth specification.

## Transformation protocol

Every transformation follows a common-mapping protocol:

1. Observations from both groups are pooled only to estimate the transformation parameters.
2. A single transformation is fitted to the pooled observations.
3. The same fitted mapping is applied identically to both groups.
4. All candidate transformations are evaluated on the same simulated samples.
5. Raw and transformed real-data results use the same bootstrap indices.

Transformations are never fitted separately within groups. Separate group-wise fitting can change between-group relationships and create misleading apparent separation.

The notebook evaluates:

1. Raw scale
2. Pooled z-score
3. Pooled min–max
4. Log plus z-score
5. Log plus min–max
6. Box–Cox plus z-score
7. Box–Cox plus min–max
8. Pooled rank-normal plus min–max

Affine transformations should leave Cohen's (d), OVL, and ESI unchanged apart from numerical precision. Nonlinear transformations can change all three components and may also change the scientific estimand.

## Important transformation caveat

A higher transformed-scale ESI does not necessarily mean that the original scientific effect has improved.

Nonlinear transformations can change:

- the mean contrast;
- relative distances among observations;
- variance and skewness;
- distributional overlap; and
- the interpretation of a one-unit difference.

A transformed-scale mean contrast can arise even when raw-scale population means are equal. Transformation decisions should therefore prioritize the scientific question, target estimand, interpretability, model assumptions, and intended application. ESI describes the consequences of a transformation; it does not determine transformation choice in isolation.

## Simulation design

The analysis examines four canonical two-group scenarios.

| Scenario | Group A | Group B | Purpose |
|---|---|---|---|
| Normal clear | (mathcal{N}(0,1)) | (mathcal{N}(1.5,1)) | Large mean difference with clear separation |
| Normal overlap | (mathcal{N}(0,1)) | (mathcal{N}(0.5,1)) | Persistent mean difference with substantial overlap |
| Skewed lognormal | Log mean 0, log SD 0.8 | Log mean 0.28, log SD 0.8 | Positive right-skewed measurements |
| Exponential vs gamma | Exponential, mean 1 | Gamma, mean 1 | Equal raw-scale means but different distributional shapes |

The sample sizes are:

```text
10, 20, 50, 100, 200, 500, and 2,000 observations per group
```

Each condition uses 500 Monte Carlo replicates. Within a replicate, the same observations are passed through every candidate transformation, creating a paired and reproducible comparison.

The exponential-versus-gamma scenario is particularly important. Its two raw-scale distributions have equal population means but different shapes and variances. A nonlinear transformation can produce a transformed-scale mean contrast in this setting. Such a change must not be interpreted as strengthening an original raw-scale mean difference that did not exist.

## Biomedical datasets

The real-data analysis contains 43 continuous-variable comparisons across eight public datasets.

| Dataset | Total sample size | Group comparison |
|---|---:|---|
| Breast Cancer Wisconsin | 569 | Malignant vs benign |
| Pima Diabetes | 768 | No diabetes vs diabetes |
| Cleveland Heart Disease | 303 | No disease vs disease |
| Framingham CHD | 4,240 | No 10-year CHD vs 10-year CHD |
| Fetal Health | 1,831 analyzed | Normal vs pathological |
| Heart Disease Prediction | 918 | No disease vs disease |
| Haberman Survival | 306 | Survived at least five years vs died within five years |
| Heart Failure Clinical Records | 299 | Survived follow-up vs died during follow-up |

Dataset-specific safeguards include:

- exclusion of the intermediate “suspect” class from the binary Fetal Health analysis;
- treatment of physiologically impossible zeros as missing for relevant Pima variables;
- variable-specific complete-case analysis;
- preservation of the original clinically defined outcome groups;
- separate treatment of Heart Disease Prediction and Heart Failure Clinical Records; and
- no artificial outcome construction through median splitting.

## Real-data bootstrap analysis

Each variable comparison uses 2,000 nonparametric bootstrap replicates. Sampling is performed with replacement separately within the two outcome groups.

The same bootstrap indices are used for the raw and transformed scales. This paired design ensures that the estimated transformation effect is not caused by different bootstrap draws.

For every bootstrap sample, the notebook calculates:

- signed Cohen's (d);
- Gaussian-kernel distributional overlap; and
- the values required to construct ESI.

The final summary includes the raw ESI, transformed ESI, and signed difference:

[
Deltamathrm{ESI}
=mathrm{ESI}_{mathrm{transformed}}
-mathrm{ESI}_{mathrm{raw}}.
]

For this study, (|Deltamathrm{ESI}|<0.05) is treated as practical equivalence. This is an operational tolerance for the paired analysis rather than a universal cutoff.

## Distributional overlap estimation

The overlap coefficient is estimated as:

[
mathrm{OVL}
=int minleft[f_A(y),f_B(y)
ight],mathrm{d}y,
]

where (f_A) and (f_B) are the estimated group densities.

The primary analysis uses:

- Gaussian kernel density estimation;
- a robust Silverman bandwidth;
- a common 512-point evaluation grid;
- density normalization; and
- trapezoidal integration of the pointwise minimum.

An OVL close to 1 indicates extensive shared density mass. An OVL close to 0 indicates limited distributional overlap.

## Sensitivity analyses

The notebook generates four supplementary analyses.

### Supplementary Figure S1 — Component decomposition

Displays median (|d|), the standard deviation of signed (d), mean OVL, and ESI across sample size. It demonstrates that the sample-size dependence of ESI is largely driven by increasing stability of the effect estimate.

### Supplementary Figure S2 — Simulation calibration

Maps ESI across population Cohen's (d) and sample size under a normal reference model. It demonstrates why universal ESI magnitude categories would be misleading.

### Supplementary Figure S3 — Kernel and bandwidth sensitivity

Compares Gaussian, top-hat, Epanechnikov, exponential, linear, and cosine kernels across bandwidth multipliers from 0.5 to 2.0 relative to the reference bandwidth.

### Supplementary Figure S4 — Stability-estimator sensitivity

Compares the standard deviation of signed (d) with scaled median absolute deviation, scaled interquartile range, and a confidence-interval-width-derived standard deviation.

## Detailed output inventory

The final ZIP archive includes:

### Main figures

```text
Figure_1_publication.png
Figure_1_publication.pdf
Figure_2_publication.png
Figure_2_publication.pdf
Figure_3_publication.png
Figure_3_publication.pdf
```

### Supplementary figures

```text
Figure_S1_component_decomposition.png
Figure_S2_simulation_calibration.png
Figure_S3_kernel_bandwidth_sensitivity.png
Figure_S4_stability_sensitivity.png
```

### Source data and tables

- simulation-level replicate data;
- simulation summaries;
- complete 43-variable real-data results;
- ESI component values;
- dataset overview and provenance manifest;
- selected clinical examples;
- figure-level source-data CSV files; and
- `Source_Data_Colab.xlsx`.

### Reproducibility records

- downloaded public datasets;
- dataset source URLs;
- SHA-256 file hashes;
- random seed and replicate settings;
- Python and package versions;
- analysis metadata; and
- required-output validation results.

## Main manuscript figures

**Figure 1** contrasts statistical evidence, standardized effect magnitude, and ESI across sample size.

**Figure 2** shows that pooled nonlinear transformations alter ESI through different combinations of effect magnitude, estimation stability, and distributional separation.

**Figure 3** presents three biomedical examples in which the same pooled rank-normal plus min–max transformation produces a substantial ESI increase, a modest increase, and a substantial decrease.

Together, these figures establish that transformation effects are variable dependent rather than universally favorable.

## Troubleshooting

### The runtime disconnected

Reconnect the Colab runtime and select **Runtime → Run all**. Partial outputs from an interrupted session should not be treated as the final analysis.

### A dataset download failed

Public file hosts can occasionally be unavailable. Restart the runtime and rerun the notebook. The dataset manifest identifies every expected source.

### The final ZIP did not download automatically

Open the Colab file browser, locate `ESI_Colab_complete_outputs.zip`, select the three-dot menu, and choose **Download**.

### The analysis takes longer than expected

Runtime varies with the allocated Colab CPU. The 2,000-replicate bootstrap analysis is intentionally computationally intensive. A GPU is not required.

## Responsible use

ESI is intended as a methodological research and reporting diagnostic. It must not be interpreted automatically as evidence of:

- causality;
- clinical benefit;
- biological importance;
- predictive accuracy;
- treatment effectiveness; or
- universal superiority of a transformed scale.

Applications should report the ESI components, preserve the meaning of the measurement scale, and provide appropriate conventional statistics and graphical displays.

## Citation

If this analysis or ESI is used, please cite the manuscript above. The journal citation and DOI will be updated after publication.

## Questions and contact

**Dr Mohamed Elgendi**  
Department of Biomedical Engineering and Biotechnology  
Centre for Biotechnology  
Khalifa University  
Abu Dhabi, United Arab Emirates  

**Email:** [mohamed.elgendi@ku.ac.ae](mailto:mohamed.elgendi@ku.ac.ae)  
**Repository:** [https://github.com/Elgendi/The-Effect-Separation-Index](https://github.com/Elgendi/The-Effect-Separation-Index)
