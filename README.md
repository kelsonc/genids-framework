# GenIDS Framework

## Overview

GenIDS Framework is an open-source experimental framework developed to support research on the generalization of Machine Learning-based Intrusion Detection Systems (ML-IDS) across different network data domains.

The framework implements the experimental pipeline developed in the doctoral research entitled:

> **Generalization of Machine Learning-Based Intrusion Detection Systems**

It provides notebooks and supporting resources for network-flow extraction, dataset customization, baseline construction, data interventions, model training, evaluation, result analysis, and experiment reproduction.

## Objectives

The framework supports:

- network-flow extraction from PCAP files using NFStream;
- customization and standardization of intrusion-detection datasets in a common feature space;
- construction of the experimental baselines;
- intra-domain and inter-domain evaluation;
- binary and multiclass classification;
- dimensionality reduction using PCA;
- feature selection using Chi-Square;
- integration of labeled network flows across data domains;
- training and evaluation of Machine Learning models for intrusion detection;
- reproduction of the experiments and results reported in the doctoral thesis.

## Framework Components

### 1. Network-flow extraction and dataset customization

The framework includes notebooks for:

- extracting bidirectional network flows from the original PCAP files using NFStream;
- processing the original UNSW-NB15, CIC-IDS2017, and CIC-IDS2018 datasets;
- selecting and organizing a common set of network-flow features;
- cleaning, labeling, balancing, and standardizing the extracted data;
- generating the customized datasets GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18.

The resulting processed datasets are distributed separately through the GenIDS Benchmark.

### 2. Experimental baselines

The baseline notebooks implement the preprocessing, training, validation, and evaluation procedures used as reference for the experiments. They cover:

- intra-domain scenarios, in which training and testing data originate from the same data domain;
- inter-domain scenarios, in which models are trained on a source domain and evaluated on a different target domain;
- binary and multiclass classification settings;
- the models and evaluation metrics adopted in the doctoral thesis.

### 3. Data-intervention experiments

The experimental notebooks are the same artifacts made available in the repository associated with the SBSeg 2026 paper. The nine notebooks evaluate network-flow integration and its combination with PCA and Chi-Square in the scenarios and proportions reported in the paper.

The experiments are organized according to the following interventions:

| Intervention | Description |
|---|---|
| IN1 | Dimensionality reduction using PCA |
| IN2 | Feature selection using Chi-Square |
| IN3 | Integration of labeled flows from the target domain |
| IN4 | Flow integration followed by PCA |
| IN5 | Flow integration followed by Chi-Square |

The flow-integration experiments consider normal, malicious, and mixed flows from the target domain. The integration proportions and other experimental parameters are preserved as reported in the corresponding study.

## Datasets

The experiments use the processed datasets made available through the [GenIDS Benchmark](https://doi.org/10.5281/zenodo.21435638):

- [GenIDS-NB15](https://doi.org/10.5281/zenodo.21435638);
- [GenIDS-CIC17](https://doi.org/10.5281/zenodo.21435638);
- [GenIDS-CIC18](https://doi.org/10.5281/zenodo.21435638).

These datasets are derived from:

- [UNSW-NB15](https://research.unsw.edu.au/projects/unsw-nb15-dataset);
- [CIC-IDS2017](https://www.unb.ca/cic/datasets/ids-2017.html);
- [CIC-IDS2018](https://www.unb.ca/cic/datasets/ids-2018.html).

The processed datasets are not stored directly in this repository. Download instructions and the expected directory structure will be provided in the reproducibility documentation.

## Repository Structure

```text
GenIDS-Framework/
|
|-- flow_extraction/
|   |-- README.md
|   `-- notebooks/
|
|-- dataset_customization/
|   |-- README.md
|   `-- notebooks/
|
|-- baselines/
|   |-- README.md
|   `-- notebooks/
|
|-- experiments/
|   |-- README.md
|   `-- notebooks/
|       |-- 01_experiment.ipynb
|       |-- 02_experiment.ipynb
|       |-- ...
|       `-- 09_experiment.ipynb
|
|-- docs/
|   |-- CODE_DOCUMENTATION.md
|   `-- REPRODUCIBILITY.md
|
|-- results/
|   `-- README.md
|
|-- requirements.txt
|-- setup_env.sh
|-- run_experiments.sh
|-- citation.bib
|-- LICENSE
`-- README.md
```

The final notebook names and their mappings to the thesis experiments, tables, and figures will be documented as the repository is consolidated.

## Reproducibility

The repository follows the organization adopted in the SBSeg 2026 artifact repository. The reproduction resources will include:

- source-code documentation;
- instructions for downloading and organizing the GenIDS Benchmark datasets;
- fixed software dependencies;
- environment-configuration instructions;
- execution scripts;
- expected outputs;
- mapping between notebooks, experiments, and reported results.

Detailed instructions will be available in `docs/REPRODUCIBILITY.md`.

## Computational Environment

### Hardware

| Component | Specification |
|---|---|
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

- pandas
- NumPy
- scikit-learn
- XGBoost
- NFStream
- Matplotlib
- Seaborn

The exact dependency versions will be provided in `requirements.txt`.

## Citation

If you use this framework in your research, please cite:

SANTOS, K. C.; MIANI, R. S. Generalization of Machine Learning-Based Intrusion Detection Systems. Doctoral Thesis. Federal University of Uberlandia, Uberlandia, Brazil, 2026.

## License

This project is released under the MIT License. See `LICENSE` for details.
