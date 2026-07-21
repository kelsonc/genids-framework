# GenIDS Framework

## Overview

GenIDS Framework is an open-source experimental framework designed to support research on the cross-dataset generalization of Machine Learning-based Intrusion Detection Systems (ML-IDS).

The framework implements the experimental developed in the doctoral research entitled:

> **Generalization of Machine Learning-Based Intrusion Detection Systems**

It provides tools for network-flow extraction, dataset preprocessing, feature engineering, data interventions, model training, evaluation, and result visualization.

## Objectives

The framework supports:

- network-flow extraction from PCAP files using NFStream;
- preprocessing and standardization of intrusion-detection datasets;
- intra-domain and inter-domain evaluation;
- dimension reduction using PCA;
- feature selection using Chi-Square;
- integration of network flows across data domains;
- training and evaluation of supervised models for intrusion detection.

## Data interventions

| Intervention | Description |
|---|---|
| IN1 | Dimensionality reduction using PCA |
| IN2 | Feature selection using Chi-Square |
| IN3 | Integration of labeled flows from the target domain |
| IN4 | Flow integration followed by PCA |
| IN5 | Flow integration followed by Chi-Square |

## Datasets

The experiments use the processed datasets made available in the [GenIDS Benchmark](https://doi.org/10.5281/zenodo.21435638).
- [GenIDS-NB15](https://doi.org/10.5281/zenodo.21435638)
- [GenIDS-CIC17](https://doi.org/10.5281/zenodo.21435638)
- [GenIDS-CIC18](https://doi.org/10.5281/zenodo.21435638)

The original datasets are derived from:

- [UNSW-NB15](https://research.unsw.edu.au/projects/unsw-nb15-dataset)
- [CIC-IDS2017](https://www.unb.ca/cic/datasets/ids-2017.html)
- [CIC-IDS2018](https://www.unb.ca/cic/datasets/ids-2018.html)

## Structure

```text
GenIDS-Framework/
│
├── README.md
│
├── 01_flow_extraction/
│   ├── README.md
│   ├── 01_unsw_nb15_flow_extraction.ipynb
│   ├── 02_cic_ids2017_flow_extraction.ipynb
│   └── 03_cic_ids2018_flow_extraction.ipynb
│
├── 02_dataset_standardization/
│   ├── README.md
│   ├── 01_genids_nb15_standardization.ipynb
│   ├── 02_genids_cic17_standardization.ipynb
│   └── 03_genids_cic18_standardization.ipynb
│
├── 03_interventions/
│   ├── README.md
│   ├── IN1_dimension_reduction/
│   │   ├── README.md
│   │   └── in1_pca.ipynb
│   ├── IN2_feature_selection/
│   │   ├── README.md
│   │   └── in2_chi_square.ipynb
│   ├── IN3_flow_integration/
│   │   ├── README.md
│   │   ├── 01_normal_flow_integration.ipynb
│   │   ├── 02_malicious_flow_integration.ipynb
│   │   └── 03_mixed_flow_integration.ipynb
│   ├── IN4_hybrid_integration_pca/
│   │   ├── README.md
│   │   ├── 01_normal_flow_integration_pca.ipynb
│   │   ├── 02_malicious_flow_integration_pca.ipynb
│   │   └── 03_mixed_flow_integration_pca.ipynb
│   └── IN5_hybrid_integration_chi_square/
│       ├── README.md
│       ├── 01_normal_flow_integration_chi_square.ipynb
│       ├── 02_malicious_flow_integration_chi_square.ipynb
│       └── 03_mixed_flow_integration_chi_square.ipynb
│
├── docs/
│   ├── installation.md
│   ├── methodology.md
│   └── reproducibility.md
│
├── requirements.txt
└── citation.bib
```

## Computational Environment

### Hardware

| Component | Specification |
|------------|-----------------------------|
| Operating System | Ubuntu 20.04.6 LTS (64-bit) |
| Kernel | Linux 5.4.0-216-generic |
| Processor | Intel Xeon E-2224G @ 3.50 GHz |
| Memory | 32 GB DDR4 |
| Storage | 2 TB (1 TB SSD + 1 TB HDD) |

### Software

- Python 3.8.10
- Jupyter Notebook
- JupyterLab

### Main Libraries

- Pandas
- NumPy
- Scikit-learn
- XGBoost
- NFStream
- Matplotlib
- Seaborn

## Citation

If you use this framework in your research, please cite:

SANTOS, K. C.; MIANI, R. S. Generalization of Machine Learning-Based Intrusion Detection Systems. Doctoral Thesis. Federal University of Uberlândia, Uberlândia, Brazil, 2026.

## License

This project is released under the MIT License.
