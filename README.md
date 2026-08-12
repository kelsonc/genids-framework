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
- dimension reduction using PCA;
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

The experimental notebooks are the same artifacts made available in the repository associated with the [SBSeg 2026 paper](https://github.com/kelsonc/paper-sbseg2026.git). The nine notebooks evaluate network-flow integration and its combination with PCA and Chi-Square in the scenarios and proportions reported in the paper.

The experiments are organized according to the following interventions:

| Intervention | Description |
|---|---|
| IN1 | Dimension reduction using PCA |
| IN2 | Feature selection using Chi-Square |
| IN3 | Integration of labeled flows from the target domain |
| IN4 | Flow integration followed by PCA |
| IN5 | Flow integration followed by Chi-Square |

The flow-integration experiments consider normal, malicious, and mixed flows from the target domain. The integration proportions and other experimental parameters are preserved as reported in the corresponding study.

## Repository Structure

```text
GenIDS-Framework/
|
|-- docs/
|   |-- CODE_DOCUMENTATION.md
|   `-- REPRODUCIBILITY.md
|
|-- flow_extraction_dataset_customization/
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
|       |-- experiment_01.ipynb
|       |-- experiment_02.ipynb
|       |-- experiment_03.ipynb
|       |-- experiment_04.ipynb
|       |-- experiment_05.ipynb
|       |-- experiment_06.ipynb
|       |-- experiment_07.ipynb
|       |-- experiment_08.ipynb
|       |-- experiment_09.ipynb
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

## Computational Environment

The experiments were originally executed in the following environment:

### Hardware

| Component | Configuration |
|---|---|
| Operating system | Ubuntu 20.04.6 LTS (64-bit), Linux kernel 5.4.0-216-generic |
| Processor | Intel(R) Xeon(R) E-2224G CPU @ 3.50 GHz |
| Memory | 32 GB DDR4 RAM |
| Storage | 2 TB (1 TB SATA SSD + 1 TB HDD) |
| Python | 3.8.10 |
| Development environment | Jupyter Notebook / JupyterLab |

This configuration describes the environment used by the authors and should not be interpreted as a strict minimum hardware requirement. The notebooks may run on systems with fewer computational resources, although execution time and memory pressure may increase depending on the dataset and experiment.

### Software

- Python 3.8.10
- Jupyter Notebook
- JupyterLab

### Main Libraries

- Pandas
- NumPy
- Scikit-learn
- XGBoost
- IForest
- NFStream
- Matplotlib
- Seaborn

The exact dependency versions will be provided in `requirements.txt`.

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

## Datasets

The experiments use the following standardized datasets:

- **GenIDS-NB15**, derived from [UNSW-NB15](https://research.unsw.edu.au/projects/unsw-nb15-dataset);;
- **GenIDS-CIC17**, derived from [CIC-IDS2017](https://www.unb.ca/cic/datasets/ids-2017.html); and
- **GenIDS-CIC18**, derived from [CIC-IDS2018](https://www.unb.ca/cic/datasets/ids-2018.html).

The processed datasets used by the experimental notebooks are available in the [GenIDS Benchmark](https://zenodo.org/records/21435638) via the Zenodo permanent record. The original public datasets are not redistributed in this GitHub repository. The GenIDS versions were generated from the original network traffic captures using NFStream, producing a common set of flow features for cross-dataset experiments. The datasets were subsequently preprocessed and labeled according to the methodology described in the paper.

The NFStream features are documented in [Features Extracted with NFStream from the PCAP Files of the Datasets](features.pdf). They include flow identification and traffic-volume information, packet-derived statistical measurements, and application-related features.

> **Note:** The dataset paths in the notebooks must point to the local directory where the files downloaded from GenIDS Benchmark are stored. Detailed configuration and execution instructions will be documented as part of the GenIDS Framework documentation.

## Experiments

Detailed instructions for reproducing the principal experimental claims, the expected results, and the complete execution workflow are provided in [`docs/REPRODUCIBILITY.md`](docs/REPRODUCIBILITY.md). After configuring the local dataset paths, the principal SeloR experiments can be executed with:

```bash
./run_experiments.sh 7 8 9
```

To execute all nine notebooks sequentially, use `./run_experiments.sh all`. Execution time was not measured as an experimental variable in the paper and may vary substantially with the available hardware.

Each notebook corresponds directly to one experiment described in the paper.

### Intervention IN3 - Network Flow Integration

- [Experiment_01](notebooks/notebook_1.ipynb): benign flow integration.
- [Experiment_02](notebooks/notebook_2.ipynb): malicious (D)DoS flow integration.
- [Experiment_03](notebooks/notebook_3.ipynb): mixed integration of benign and malicious (D)DoS flows.

Experiments 1-3 evaluate integration percentages of 20%, 40%, 60%, and 80% across Interset combinations of GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18.

### Intervention IN4 - PCA + Network Flow Integration

Principal Component Analysis (PCA) is combined with the corresponding flow-integration strategy. In the experiments reported in the paper, PCA transforms the common feature space from 70 original features to 25 principal components.

- [Experiment_04](notebooks/notebook_4.ipynb): PCA + benign flow integration.
- [Experiment_05](notebooks/notebook_5.ipynb): PCA + malicious (D)DoS flow integration.
- [Experiment_06](notebooks/notebook_6.ipynb): PCA + mixed flow integration.

### Intervention IN5 - Chi-Square Feature Selection + Network Flow Integration

Chi-Square feature selection is combined with the corresponding flow-integration strategy. The feature-selection step reduces the common feature space from 70 to 25 selected features.

- [Experiment_07](notebooks/notebook_7.ipynb): Chi-Square + benign flow integration.
- [Experiment_08](notebooks/notebook_8.ipynb): Chi-Square + malicious (D)DoS flow integration.
- [Experiment_09](notebooks/notebook_9.ipynb): Chi-Square + mixed flow integration.

Together, these artifacts provide the code, standardized data references, and supplementary feature documentation associated with the experimental evaluation presented in the paper.

## Citation

If you use this framework in your research, please cite:

SANTOS, K. C.; MIANI, R. S. Generalization of Machine Learning-Based Intrusion Detection Systems. Doctoral Thesis. Federal University of Uberlandia, Uberlandia, Brazil, 2026.

## License

The source code, scripts, and Jupyter notebooks in this repository are distributed under the [MIT License](LICENSE).

The processed datasets available through [GenIDS Benchmark](https://zenodo.org/records/21435638) are distributed under the Creative Commons Attribution 4.0 International (CC BY 4.0) license.
