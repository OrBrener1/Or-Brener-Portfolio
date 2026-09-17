# Machine Learning Research for Cancer Survival & Clinical Outcome Prediction

**Dr. Alon Bartal's Lab, Bar-Ilan University | In collaboration with the University of Miami**
2025–2026

## Overview

This project focused on predicting survival and clinical outcomes in cancer patients using real-world biomedical data from the NIH **All of Us Research Program** — a national-scale database of clinical, demographic, and longitudinal patient records.

I co-led this research project, working across the full pipeline: data exploration and preprocessing, feature engineering, model development, and evaluation.

## What I did

- Co-led a machine learning research project developing predictive models for cancer survival and clinical outcomes using real-world biomedical data from the NIH All of Us Research Program.
- Developed, evaluated, and compared statistical and graph-based machine learning models, including Graph Neural Networks (GNNs), to assess predictive performance and identify robust approaches for patient outcome prediction.
- Gained hands-on experience working with sensitive real-world patient health data in a secure, privacy-compliant research environment, under strict data-access and governance requirements.
- Collaborated with researchers from Bar-Ilan University and the University of Miami on scientific discussions, research decisions, and presentation of project findings.

## Approach

Three modeling approaches were built and compared:

1. **Tabular Cox baseline** — a classical survival model using structured clinical features.
2. **Intermediate GNN model** — a graph-based approach incorporating relational structure between patients and clinical variables.
3. **Full knowledge-graph GNN model (HeteroGraphSAGE-Cox)** — a heterogeneous graph neural network combining multiple data sources into a unified survival model.

Models were evaluated using industry-standard survival analysis metrics, including the Harrell and IPCW concordance index (C-index), Kaplan-Meier survival curves, time-dependent AUC, and stratified cross-validation.

## Data & tools

- **Data sources:** NIH All of Us Research Program, integrated with external clinical datasets (SCAN360)
- **Tools & libraries:** Python, PyTorch / PyTorch Geometric, pandas, SQL / BigQuery, lifelines / scikit-survival

## A note on the code

The underlying code lives in our lab's private research repository and is not included here. This is because the project uses real, identifiable patient health data through the All of Us Researcher Workbench, which — under the program's data use policy — cannot be exported or shared outside its secure research environment. This README describes the project's goals, methods, and my contributions in place of the code itself.
