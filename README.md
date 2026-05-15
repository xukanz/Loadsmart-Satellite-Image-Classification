# Loadsmart Warehouse Identification Pipeline

This repository contains a geospatial data construction, satellite imagery retrieval, and image classification pipeline developed for the MIT SCM.256 final project sponsored by Loadsmart.

The project investigates whether publicly available geospatial information and satellite imagery can support scalable warehouse identification and logistics-oriented lead generation workflows. The pipeline combines sponsor-provided warehouse coordinates, OpenStreetMap-derived negative samples, YOLO-based image classification, calibration, and downstream ranking workflows to evaluate warehouse-like commercial properties from satellite imagery.

---

## Project Motivation

Warehouse location intelligence can support:
- logistics network analysis;
- lead generation;
- supply-chain visibility;
- geospatial database enrichment.

Because the sponsor data contain only known warehouse locations, the project focuses heavily on realistic negative-sample construction using OpenStreetMap and geospatial sampling strategies.

---

## Repository Structure

```text
Loadsmart-Warehouse-Pipeline/
│
├── Code/
│   │
│   ├── Notebook0_Initial_Sampling.ipynb
│   │   Exploratory comparison of early negative-sampling strategies
│   │
│   ├── Notebook1_Sampling_Pipeline.ipynb
│   │   Finalized negative-sampling and dataset-construction workflow
│   │
│   ├── Notebook2_Finalized_Pipeline.ipynb
│   │   Main model training, calibration, and evaluation pipeline
│   │
│   ├── Notebook3_Evaluation_Pipeline.ipynb
│   │   Operational evaluation, ranking metrics, and lead-generation workflows
│   │
│   ├── Notebook4_Ablation.ipynb
│   │   Ablation studies across architectures and training configurations
│
├── Output/
│   │
│   ├── Notebook_0/
│   ├── Notebook_1/
│   ├── Notebook_2/
│   ├── Notebook_3/
│   └── Notebook_4/
│
├── Final_Report.pdf
│
└── README.md
```

---

## Pipeline Overview

The repository implements the following workflow:

1. Positive Warehouse Processing
   - Clean and de-duplicate warehouse coordinates
   - Characterize warehouse density using DBSCAN clustering

2. Negative Sample Construction
   - Query OpenStreetMap categories around warehouse seeds
   - Apply exclusion buffers and coordinate de-duplication
   - Construct soft-negative and hard-negative datasets

3. Satellite Image Retrieval
   - Download Google satellite tiles
   - Build train/validation/test datasets

4. Model Training
   - Train YOLOv8 classification models
   - Train ResNet-18 baselines
   - Compare regularization, augmentation, and split strategies

5. Calibration and Evaluation
   - Apply temperature scaling
   - Tune deployment-oriented thresholds
   - Evaluate PR-AUC, recall, precision, and macro-F1

6. Operational Ranking and Lead Generation
   - Compute Precision@K, Recall@K, and Lift@K
   - Generate warehouse lead tables
   - Aggregate city-level opportunity metrics
   - Score external commercial-building candidates

---

## Dataset Summary

The finalized benchmark dataset contains:
- 4,205 unique warehouse locations;
- 1,528 constructed non-warehouse locations;
- 5,733 total observations.

Negative examples include both general non-warehouse categories and warehouse-like hard negatives such as industrial, commercial, factory, and wholesale regions.

Both random and spatial train-validation-test splits were generated to evaluate geographic leakage effects.

---

## Models Evaluated

The primary evaluated models include:
- YOLOv8s image classification models;
- ResNet-18 baselines.

Experiments compare:
- negative-sampling strategy;
- regularization strength;
- label smoothing;
- offline augmentation;
- spatial vs random splitting.

The retained classifier is based on the YOLOv8s architecture trained on the `density_v2` dataset configuration.

---

# External APIs and Data Sources

This project uses:
- Google Static Maps API for satellite imagery retrieval;
- OpenStreetMap / OSMnx for negative-sample construction;
- Overture Maps for candidate-discovery experiments;
- Natural Earth populated-place data for regional aggregation.

Users reproducing the image-download stages should configure their own Google Maps API key before executing retrieval workflows.

Additional execution notes are provided directly within the notebooks.

---

## Reproducibility Notes

The repository is organized into modular notebook stages covering:
- dataset construction;
- image retrieval;
- model training;
- calibration;
- evaluation;
- lead generation;
- ablation analysis.

Intermediate metadata tables, model artifacts, calibration outputs, and ranking results are archived in the corresponding output directories.

Because OpenStreetMap queries depend on live public data, archived metadata CSV files should be treated as the canonical benchmark inputs for reproducing reported experiments.

---

## Important Notes

This repository represents a research-oriented benchmark and operational prototype rather than a production-ready warehouse detection system.

Performance should be interpreted within the context of the constructed dataset and evaluation procedures described in the final report.

---

## Authors

Developed as part of the _MIT SCM.256_ course project sponsored by Loadsmart.

Authors:
- Jasper Liu
- Lavinia Xu
- Iris Zhang
- Zi Zhu
