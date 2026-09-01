Effect Separation Index
![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)
This repository contains the complete reproducible analysis for:
> Elgendi, M. *The Effect Separation Index: A Composite Diagnostic for Evaluating Group Separation Across Measurement Scales.*
The repository is intentionally minimal. The single notebook downloads the public datasets, executes the simulations and real-data analyses, generates every main and supplementary figure, creates the manuscript tables and source-data workbook, validates the required outputs, and packages the results.
Files
`ESI_complete_Colab_analysis.ipynb` — self-contained Google Colab analysis.
`README.md` — documentation and execution instructions.
No local data upload is required. The notebook retrieves the public datasets used in the study and records their source URLs and SHA-256 hashes.
Run in Google Colab
Click Open in Colab above.
In Colab, select Runtime → Run all.
Accept the browser download when the final cell creates `ESI_Colab_complete_outputs.zip`.
A standard CPU runtime is sufficient. The complete run may take approximately 10–20 minutes because the real-data analysis uses 2,000 within-group bootstrap replicates and the simulations use 500 paired Monte Carlo replicates per condition.
Analysis specification
The notebook implements the final analysis reported in the manuscript:
transformation parameters are estimated from pooled observations;
the same fitted mapping is applied to both groups;
every candidate transformation is evaluated on identical simulated samples;
real-data raw and transformed results use identical bootstrap indices;
the primary overlap estimate uses Gaussian kernel density estimation with a robust Silverman bandwidth;
ESI uses the median absolute Cohen's (d), the standard deviation of the signed resampled (d), and mean distributional overlap;
transformation effects are reported as
(\Delta\mathrm{ESI}=\mathrm{ESI}{\mathrm{transformed}}-\mathrm{ESI}{\mathrm{raw}});
ESI is treated as a sample-dependent diagnostic, not an intrinsic population effect size or a universal transformation optimizer.
Generated outputs
Running the notebook creates `ESI_Colab_complete_outputs.zip`, containing:
Figures 1–3 in publication-quality PNG and PDF formats;
Supplementary Figures S1–S4;
complete figure-level source-data CSV files;
the 43-variable real-data results;
the eight-dataset inventory and direct source links;
`Source_Data_Colab.xlsx`, with the journal source data;
downloaded public datasets and provenance information;
analysis metadata, random seed, package versions, and validation results.
Effect Separation Index
For resampled effect estimates (d^{(b)}) and overlap estimates (\mathrm{OVL}^{(b)}),
[
\mathrm{ESI}=
\frac{\operatorname{median}(|d^{(b)}|)}
{\operatorname{sd}(d^{(b)})}
\left[1-\operatorname{mean}(\mathrm{OVL}^{(b)})\right].
]
The denominator is calculated from the signed effect estimates. ESI depends on sample size and the resampling and density-estimation specifications. It should be interpreted with Cohen's (d), overlap, uncertainty, graphical displays, and scientific context.
Reproducibility
Master random seed: `2025`
Monte Carlo replicates per simulation condition: `500`
Real-data bootstrap replicates: `2,000`
KDE evaluation grid: `512` points
Primary environment: Python 3.12
The notebook installs its Python dependencies in the first executable cell. For the version of record, use the notebook associated with the manuscript submission or archived software release.
Data
All analyzed datasets are publicly available. The generated dataset manifest records the exact download location, analyzed groups and variables, sample counts, missing-data handling, and file hashes. The 918-record Heart Disease Prediction dataset and the 299-record Heart Failure Clinical Records dataset are treated as distinct datasets. No artificial median split is used.
Citation
If this analysis or ESI is used, please cite the manuscript above. Replace this section with the journal citation and DOI after publication.
Contact
Mohamed Elgendi  
Khalifa University, Abu Dhabi, United Arab Emirates  
mohamed.elgendi@ku.ac.ae
