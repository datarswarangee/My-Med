# My Med — AI-Powered Breast Cancer Molecular Subtype Classification

MyMedAI is a research-oriented machine learning project that investigates the classification of breast cancer molecular subtypes using gene expression data. It uses publicly available TCGA breast cancer data to compare machine learning models and provides a dashboard for exploring model outputs and evaluation results.

**Project status:** Internal model evaluation completed; independent external validation is in progress.

> **Research disclaimer:** MyMedAI is an educational and research prototype. It is not a clinically validated diagnostic system and must not be used to make medical decisions, recommend treatments, or replace professional medical assessment.

## Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [Molecular Subtypes](#molecular-subtypes)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Current Results](#current-results)
- [Dashboard](#dashboard)
- [External Validation](#external-validation)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Running the Project](#running-the-project)
- [Limitations](#limitations)
- [Reproducibility](#reproducibility)
- [Ethical Considerations](#ethical-considerations)
- [References](#references)
- [Author](#author)

## Overview

Breast cancer comprises biologically distinct molecular subtypes with different gene expression patterns. MyMedAI explores whether supervised machine learning can identify these subtypes from gene expression measurements.

The project focuses on:

- Preparing public gene expression and subtype-label datasets.
- Comparing Logistic Regression and Random Forest classifiers.
- Evaluating performance using class-sensitive metrics.
- Investigating selected gene features.
- Building a dashboard to present analysis results.
- Evaluating generalizability on an independent public dataset.

## Objectives

1. Develop a reproducible gene expression classification pipeline.
2. Compare multiple machine learning algorithms using patient-grouped cross-validation.
3. Evaluate performance using accuracy, balanced accuracy, macro F1, and weighted F1.
4. Examine performance across individual molecular subtypes.
5. Investigate the feasibility of independent external validation.
6. Present results through a research dashboard.

## Molecular Subtypes

The dataset uses five subtype categories:

| Label | Description |
|---|---|
| LumA | Luminal A |
| LumB | Luminal B |
| Basal | Basal-like |
| Her2 | HER2-enriched |
| Normal | Normal-like |

These are molecular expression-based categories. They should not be interpreted as direct equivalents of clinical diagnoses or treatment recommendations.

## Dataset

### Primary dataset

The project uses publicly available breast cancer gene expression and subtype-label data derived from The Cancer Genome Atlas (TCGA).

**Current processed candidate dataset:**

- 956 labelled tissue samples
- 20,530 gene-expression features
- 844 unique patients
- Five subtype classes

| Subtype | Sample count |
|---|---:|
| LumA | 434 |
| LumB | 194 |
| Basal | 142 |
| Normal | 119 |
| Her2 | 67 |
| **Total** | **956** |

Samples from the same patient may be correlated. The evaluation therefore uses patient-grouped cross-validation to reduce the risk of information leakage between training and validation folds.

**Data source:** [The Cancer Genome Atlas (TCGA)](https://www.cancer.gov/ccg/research/genome-sequencing/tcga)

## Methodology

The current workflow includes:

1. **Data preparation:** Align expression features with molecular subtype labels and verify sample consistency.
2. **Patient grouping:** Keep samples from the same patient in the same cross-validation fold.
3. **Preprocessing:** Apply the fitted pipeline's feature filtering, feature selection, and scaling steps.
4. **Model training:** Compare Logistic Regression and Random Forest classifiers.
5. **Evaluation:** Use five-fold stratified group cross-validation.
6. **Feature selection:** Evaluate a Logistic Regression pipeline selecting 500 features.
7. **Reporting:** Save evaluation results and expose relevant outputs through the dashboard.

The fitted Logistic Regression pipeline includes constant-feature removal, feature selection, scaling, and classification.

## Current Results

The following are the existing five-fold grouped cross-validation results on the primary dataset.

| Metric | Logistic Regression | Random Forest |
|---|---:|---:|
| Accuracy | 0.9174 ± 0.0223 | 0.8944 ± 0.0225 |
| Balanced accuracy | 0.9184 ± 0.0292 | 0.9001 ± 0.0322 |
| Macro F1 | 0.9158 ± 0.0216 | 0.8955 ± 0.0254 |
| Weighted F1 | 0.9179 ± 0.0222 | 0.8953 ± 0.0230 |

### Interpretation

Logistic Regression achieved higher mean performance than Random Forest across the reported metrics in this experiment. Balanced accuracy and macro F1 are particularly useful because they account for performance across classes rather than allowing larger classes to dominate the assessment.

These results describe internal cross-validation performance only. They do not establish clinical validity or guarantee performance on data from other institutions or measurement platforms.

## Dashboard

The project includes a dashboard implemented in `app/dashboard.py`.

The dashboard is intended to make the model's analysis outputs easier to inspect and interpret. Its current implementation should be treated as a research interface, not a clinical decision-support system.

To run the dashboard, use the launch command appropriate to the installed framework and the existing project configuration.

## External Validation

Independent evaluation is currently being investigated.

The project has inspected **GSE115577**, a breast cancer expression dataset based on Affymetrix microarrays. However, its measurement platform and preprocessing differ from the TCGA RNA-seq data used by the current model. Directly applying the model to these values would not constitute a defensible external validation without additional justification.

The project is therefore investigating **GSE96058** as another candidate dataset. Its sample metadata and available expression files are being inspected before selecting the appropriate data and evaluation procedure.

External validation will only be reported after:

- Expression measurements and gene identifiers have been checked.
- Subtype labels have been matched to the correct samples.
- Preprocessing and measurement-platform compatibility have been assessed.
- Patient overlap and potential data leakage have been considered.
- Evaluation metrics and limitations have been documented.

**No external-validation performance is claimed at this stage.**

## Project Structure

The following is a simplified representation of the main project directories. Individual files may differ as development progresses.

```text
MyMedAI/
├── app/
│   └── dashboard.py
├── data/
│   ├── raw/
│   ├── processed/
│   ├── processed_candidate/
│   └── external/
│       ├── GSE115577/
│       └── GSE96058/
├── models/
│   ├── baseline_logistic_regression.joblib
│   └── explainable_logistic_regression.joblib
├── reports/
│   └── external_validation/
├── src/
├── .venv/
└── README.md
```

The `.venv/` directory is a local Python environment and should not be committed to GitHub.

## Installation

### Requirements

- Windows, macOS, or Linux
- Python 3.11 (the current development environment uses Python 3.11)
- Git
- Required Python packages from the project's dependency list

### Setup on Windows

Open PowerShell in the project directory.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

Install dependencies using the project's dependency file if available:

```powershell
python -m pip install -r requirements.txt
```

If `requirements.txt` has not yet been created, generate and verify a project-specific dependency list before following this installation step.

## Running the Project

Activate the virtual environment:

```powershell
.\.venv\Scripts\Activate.ps1
```

Launch the dashboard using the command supported by the existing application framework. For example, if the application uses Streamlit:

```powershell
python -m streamlit run app/dashboard.py
```

Use this example only if Streamlit is the framework actually used by `app/dashboard.py`.

Model retraining and evaluation should use the project's existing scripts and verified data paths. Do not retrain or overwrite saved models unless the experiment is intended to do so.

## Limitations

- The project is a research prototype, not a clinically validated medical device.
- Subtype labels depend on the source dataset and its annotation methodology.
- Multiple samples from a patient require grouped evaluation to avoid leakage.
- Internal cross-validation is not a substitute for independent external validation.
- Differences in expression platforms and normalization can substantially affect model transferability.
- Selected genes are model features and should not automatically be interpreted as causal biomarkers.
- Treatment response, prognosis, and treatment recommendations are outside the scope of the current validated functionality.

## Reproducibility

For reproducible experiments:

- Record the Python and package versions.
- Keep patient grouping and random seeds consistent.
- Preserve the original dataset provenance and preprocessing steps.
- Store evaluation metrics and class-level results.
- Avoid committing raw data or sensitive information without checking the relevant data-use terms.
- Record any changes to feature selection, model configuration, or validation methodology.

Saved model files may depend on specific versions of scikit-learn and other libraries. Use a tested dependency specification when loading them in a fresh environment.

## Ethical Considerations

Medical AI research requires careful handling of uncertainty, dataset bias, class imbalance, and generalizability. Results from public datasets may not transfer to different populations, institutions, or clinical settings.

MyMedAI should be used for educational exploration and research methodology development only. Any future clinical application would require appropriate independent validation, clinical review, regulatory assessment, and evaluation of safety and fairness.

## References

1. **The Cancer Genome Atlas (TCGA).** National Cancer Institute.  
   https://www.cancer.gov/ccg/research/genome-sequencing/tcga

2. **Gene Expression Omnibus (GEO).** National Center for Biotechnology Information.  
   https://www.ncbi.nlm.nih.gov/geo/

3. **GSE115577.** Tumor and tumour-adjacent gene expression in the Nurses' Health Study cohorts.  
   https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE115577

4. **GSE96058.** Breast cancer gene expression dataset.  
   https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE96058

## Author

**MyMedAI — Breast Cancer Molecular Subtype Classification**

Developed as an educational and research project exploring machine learning, gene expression analysis, and responsible evaluation of medical AI systems.

For questions, suggestions, or research collaboration, open an issue in the project repository.
