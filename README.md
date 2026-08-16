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
- inter-domain evaluation;
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

The baseline notebooks implement the preprocessing, training, validation, and evaluation procedures used as references for the experiments reported in the doctoral thesis.

The baselines cover the three standardized datasets:

- GenIDS-NB15;
- GenIDS-CIC17; and
- GenIDS-CIC18.

Two Machine Learning algorithms are evaluated:

- **XGBoost**, applied to binary and multiclass classification; and
- **Isolation Forest**, applied to no-supervised binary anomaly detection.

For each baseline, one dataset is used as the source training domain. The model is first evaluated on an intradomain hold-out partition and subsequently on the other two datasets in interdomain generalization scenarios.

The notebooks preserve the preprocessing procedures, data partitions, hyperparameters, evaluation metrics, and random seed adopted in the thesis. 

The reported metrics include Accuracy, Macro Precision, Macro Recall, Macro F1-score, AUC-ROC, and False Alarm Rate (FAR).

### 3. Data-intervention experiments

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
|-- baselines/
|   |-- genids_cic17/
|       |-- 1.cic17_xgb_baseline_binary.ipynb
|       |-- 2.cic17_iforest_baseline_binary.ipynb
|   `   |-- 3.cic17_xgb_baseline_multiclass.ipynb
|   |-- genids_cic18/
|       |-- 1.cic18_xgb_baseline_binary.ipynb
|       |-- 2.cic18_iforest_baseline_binary.ipynb
|       |-- 3.cic18_xgb_baseline_multiclass.ipynb
|   |-- genids_unsw15/
|       |-- 1.unsw_xgb_baseline_binary.ipynb
|       |-- 2.unsw_iforest_baseline_binary.ipynb
|   `   |-- 3.unsw_xgb_baseline_multiclass.ipynb
|   |-- README.md
|
|-- docs/
|   |-- CODE_DOCUMENTATION.md
|   |-- REPRODUCIBILITY.md
|
|-- experiments/
|   |-- notebooks/
|       |-- in1/
|           |-- 1.unsw_xgb_pca.ipynb
|           |-- 2.cic17_xgb_pca.ipynb
|           |-- 3.cic18_xgb_pca.ipynb
|           |-- 4.unsw_iforest_pca.ipynb
|           |-- 5.cic17_iforest_pca.ipynb
|           |-- 6.cic18_iforest_pca.ipynb
|       |-- in2/
|           |-- 1.unsw_xgb_chisquare.ipynb
|           |-- 2.cic17_xgb_chisquare.ipynb
|           |-- 3.cic18_xgb_chisquare.ipynb
|           |-- 4.unsw_iforest_chisquare.ipynb
|           |-- 5.cic17_iforest_chisquare.ipynb
|           |-- 6.cic18_iforest_chisquare.ipynb
|       |-- in3/
|           |-- experiment_in3_1.ipynb
|           |-- experiment_in3_2.ipynb
|           |-- experiment_in3_3.ipynb
|       |-- in4/
|           |-- experiment_in4_1.ipynb
|           |-- experiment_in4_2.ipynb
|           |-- experiment_in4_3.ipynb
|       |-- in5/
|           |-- experiment_in5_1.ipynb
|           |-- experiment_in5_2.ipynb
|           `-- experiment_in5_3.ipynb
|   |-- README.md
|
|-- flow_extraction/
|   |-- notebooks/
|       |-- genids_cic17/
|           |-- 1_extraction_flows_cic17_monday.ipynb
|           |-- 2_extraction_flows_cic17_tuesday.ipynb
|           |-- 3_extraction_flows_cic17_wednesday.ipynb
|           |-- 4_extraction_flows_cic17_thursday.ipynb
|           |-- 5_extraction_flows_cic17_friday.ipynb
|           |-- 6_full_flows_cic17.ipynb
|       |-- genids_cic18/
|           |-- 1_extraction_flows_cic18_tuesday.ipynb
|           |-- 2_extraction_flows_cic18_wednesday.ipynb
|           |-- 3_extraction_flows_cic18_wednesday.ipynb
|           |-- 4_extraction_flows_cic18_wednesday.ipynb
|           |-- 5_extraction_flows_cic18_thursday.ipynb
|           |-- 6_extraction_flows_cic18_thursday.ipynb
|           |-- 7_extraction_flows_cic18_thursday.ipynb
|           |-- 8_extraction_flows_cic18_friday.ipynb
|           |-- 9_extraction_flows_cic18_friday.ipynb
|           |-- 10_extraction_flows_cic18_friday.ipynb
|           |-- 11_full_flows_cic18.ipynb
|       |-- genids_unsw15/
|           |-- 1_extraction_daily_flows_unsw.ipynb
|           |-- 2_full_flows_unsw.ipynb
|   |-- README.md
|   |-- features.pdf
|
|-- LICENSE
|-- README.md
|-- citation.bib
|-- requirements.txt
```

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
- Scikit-learn (including Isolation Forest)
- XGBoost
- NFStream
- Matplotlib

The exact dependency versions are provided in `requirements.txt`.

## Reproducibility

The reproduction resources include:

- source-code documentation;
- instructions for downloading and organizing the GenIDS Benchmark datasets;
- fixed software dependencies;
- environment-configuration instructions;
- execution scripts;
- mapping between notebooks and experiments.

Detailed instructions are available in `docs/REPRODUCIBILITY.md`.

## Datasets

The experiments use the following standardized datasets:

- **[GenIDS-UNSW15](https://zenodo.org/records/21435638)**, derived from [UNSW-UNSW15](https://research.unsw.edu.au/projects/unsw-nb15-dataset);
- **[GenIDS-CIC17](https://zenodo.org/records/21435638)**, derived from [CIC-IDS2017](https://www.unb.ca/cic/datasets/ids-2017.html); and
- **[GenIDS-CIC18](https://zenodo.org/records/21435638)**, derived from [CIC-IDS2018](https://www.unb.ca/cic/datasets/ids-2018.html).

The processed datasets used by the experimental notebooks are available in the [GenIDS Benchmark](https://zenodo.org/records/21435638) via the Zenodo permanent record. The original public datasets are not redistributed in this GitHub repository. The GenIDS versions were generated from the original network traffic captures using NFStream, producing a common set of flow features for interdomain generalization. The datasets were subsequently preprocessed and labeled according to the methodology described in the study.

The NFStream features are documented in [Features Extracted with NFStream from the PCAP Files of the Datasets](flow_extraction/features.pdf).

> **Note:** The dataset paths in the notebooks must point to the local directory where the files downloaded from GenIDS Benchmark are stored. Detailed configuration and execution instructions to be documented as part of the GenIDS Framework documentation.

## Experiments

Detailed instructions for reproducing the principal experimental claims and the complete execution workflow are provided in [`docs/REPRODUCIBILITY.md`](docs/REPRODUCIBILITY.md). Execution time was not measured as an experimental variable in the study and may vary substantially with the available hardware.

Each notebook implements one experimental configuration described in the doctoral thesis. The repository contains 21 experiment notebooks organized according to interventions IN1-IN5.

Interventions IN3, IN4, and IN5 correspond to the nine experiments made available with the SBSeg 2026 paper artifacts. These experiments use XGBoost and evaluate the integration of labeled target-domain flows at 20%, 40%, 60%, and 80%.

### Intervention IN1: PCA-Based Dimension Reduction

Principal Component Analysis (PCA) is applied as an isolated intervention. In the experiments reported in the thesis, PCA transforms the common feature space from 70 original features to 25 principal components.

The intervention is evaluated with XGBoost and Isolation Forest using GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18 as source domains.

- [PCA + XGBoost — GenIDS-UNSW15](experiments/notebooks/in1/1.unsw_xgb_pca.ipynb)
- [PCA + XGBoost — GenIDS-CIC17](experiments/notebooks/in1/2.cic17_xgb_pca.ipynb)
- [PCA + XGBoost — GenIDS-CIC18](experiments/notebooks/in1/3.cic18_xgb_pca.ipynb)
- [PCA + Isolation Forest — GenIDS-UNSW15](experiments/notebooks/in1/4.unsw_iforest_pca.ipynb)
- [PCA + Isolation Forest — GenIDS-CIC17](experiments/notebooks/in1/5.cic17_iforest_pca.ipynb)
- [PCA + Isolation Forest — GenIDS-CIC18](experiments/notebooks/in1/6.cic18_iforest_pca.ipynb)

### Intervention IN2: Chi-Square Feature Selection

Chi-Square is applied as an isolated feature-selection intervention. In the experiments reported in the thesis, the method reduces the common feature space from 70 to 25 selected features.

The intervention is evaluated with XGBoost and Isolation Forest using GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18 as source domains.

- [Chi-Square + XGBoost — GenIDS-UNSW15](experiments/notebooks/in2/1.unsw_xgb_chisquare.ipynb)
- [Chi-Square + XGBoost — GenIDS-CIC17](experiments/notebooks/in2/2.cic17_xgb_chisquare.ipynb)
- [Chi-Square + XGBoost — GenIDS-CIC18](experiments/notebooks/in2/3.cic18_xgb_chisquare.ipynb)
- [Chi-Square + Isolation Forest — GenIDS-UNSW15](experiments/notebooks/in2/4.unsw_iforest_chisquare.ipynb)
- [Chi-Square + Isolation Forest — GenIDS-CIC17](experiments/notebooks/in2/5.cic17_iforest_chisquare.ipynb)
- [Chi-Square + Isolation Forest — GenIDS-CIC18](experiments/notebooks/in2/6.cic18_iforest_chisquare.ipynb)

### Intervention IN3: Network Flow Integration

- [Experiment_IN3_1](notebooks/notebook_in3_1.ipynb): benign flow integration.
- [Experiment_IN3_2](notebooks/notebook_in3_2.ipynb): malicious (D)DoS flow integration.
- [Experiment_IN3_3](notebooks/notebook_in3_3.ipynb): mixed integration of benign and malicious (D)DoS flows.

Experiments 1-3 evaluate integration percentages of 20%, 40%, 60%, and 80% across Interset combinations of GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18.

### Intervention IN4: PCA + Network Flow Integration

Principal Component Analysis (PCA) is combined with the corresponding flow-integration strategy. In the experiments reported in the study, PCA transforms the common feature space from 70 original features to 25 principal components.

- [Experiment_IN4_1](notebooks/notebook_in4_1.ipynb): PCA + benign flow integration.
- [Experiment_IN4_2](notebooks/notebook_in4_2.ipynb): PCA + malicious (D)DoS flow integration.
- [Experiment_IN4_3](notebooks/notebook_in4_3.ipynb): PCA + mixed flow integration.

### Intervention IN5: Chi-Square Feature Selection + Network Flow Integration

Chi-Square feature selection is combined with the corresponding flow-integration strategy. The feature-selection step reduces the common feature space from 70 to 25 selected features.

- [Experiment_IN5_1](notebooks/notebook_in5_1.ipynb): Chi-Square + benign flow integration.
- [Experiment_IN5_2](notebooks/notebook_in5_2.ipynb): Chi-Square + malicious (D)DoS flow integration.
- [Experiment_IN5_3](notebooks/notebook_in5_3.ipynb): Chi-Square + mixed flow integration.

Together, these artifacts provide the code, standardized data references, and supplementary feature documentation associated with the experimental evaluation presented in the study.

## Citation

If you use this framework in your research, please cite:

SANTOS, K. C.; MIANI, R. S. GenIDS-Framework: Generalization of Machine Learning-Based Intrusion Detection Systems. Software. In: Generalization of Machine Learning-Based Intrusion Detection Systems. Doctoral Thesis. Federal University of Uberlandia, Uberlandia, Brazil, 2026.

## License

The source code, scripts, and Jupyter notebooks in this repository are distributed under the [MIT License](LICENSE).

The processed datasets available through [GenIDS Benchmark](https://zenodo.org/records/21435638) are distributed under the Creative Commons Attribution 4.0 International (CC BY 4.0) license.
