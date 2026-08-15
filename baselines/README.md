# Experimental Baselines

## Overview

This directory contains the baseline notebooks used to evaluate the generalization of Machine Learning-based Intrusion Detection Systems across the GenIDS data domains.

The baselines use the standardized datasets distributed through the [GenIDS Benchmark](https://zenodo.org/records/21435638):

- **GenIDS-NB15**, derived from UNSW-NB15;
- **GenIDS-CIC17**, derived from CIC-IDS2017; and
- **GenIDS-CIC18**, derived from CIC-IDS2018.

Each notebook trains a model on one source dataset, evaluates it on an intradomain hold-out partition, and then evaluates the same fitted preprocessing pipeline and model on the other two datasets. The latter evaluations represent the interdomain generalization scenarios.

## Baseline Configurations

The repository provides nine baseline notebooks:

| Source dataset | Algorithm | Classification | Intradomain test | Interdomain tests |
|---|---|---|---|---|
| GenIDS-NB15 | XGBoost | Binary | GenIDS-NB15 | GenIDS-CIC17 and GenIDS-CIC18 |
| GenIDS-NB15 | Isolation Forest | Binary | GenIDS-NB15 | GenIDS-CIC18 and GenIDS-CIC17 |
| GenIDS-NB15 | XGBoost | Multiclass | GenIDS-NB15 | GenIDS-CIC17 and GenIDS-CIC18 |
| GenIDS-CIC17 | XGBoost | Binary | GenIDS-CIC17 | GenIDS-NB15 and GenIDS-CIC18 |
| GenIDS-CIC17 | Isolation Forest | Binary | GenIDS-CIC17 | GenIDS-NB15 and GenIDS-CIC18 |
| GenIDS-CIC17 | XGBoost | Multiclass | GenIDS-CIC17 | GenIDS-NB15 and GenIDS-CIC18 |
| GenIDS-CIC18 | XGBoost | Binary | GenIDS-CIC18 | GenIDS-NB15 and GenIDS-CIC17 |
| GenIDS-CIC18 | Isolation Forest | Binary | GenIDS-CIC18 | GenIDS-NB15 and GenIDS-CIC17 |
| GenIDS-CIC18 | XGBoost | Multiclass | GenIDS-CIC18 | GenIDS-NB15 and GenIDS-CIC17 |

XGBoost is evaluated in both binary and multiclass settings. Isolation Forest is evaluated only as a binary, no-supervised anomaly detector because its output distinguishes normal observations from anomalous observations rather than assigning attack-category labels.

## Directory Structure

```text
baselines/
|-- README.md
`-- notebooks/
    |-- genids_unsw15/
    |   |-- 1.unsw_xgb_baseline_binary.ipynb.ipynb
    |   |-- 2.unsw_iforest_baseline_binary.ipynb
    |   `-- 3.unsw_xgb_baseline_multiclass.ipynb
    |
    |-- genids_cic17/
    |   |-- 1.cic17_xgb_baseline_binary.ipynb
    |   |-- 2.cic17_iforest_baseline_binary.ipynb
    |   `-- 3.cic17_xgb_baseline_multiclass.ipynb
    |
    `-- genids_cic18/
        |-- 1.cic18_xgb_baseline_binary.ipynb
        |-- 2.cic18_iforest_baseline_binary.ipynb
        `-- 3.cic18_xgb_baseline_multiclass.ipynb
```

### Notebook index

| Source dataset | Binary XGBoost | Binary Isolation Forest | Multiclass XGBoost |
|---|---|---|---|
| GenIDS-NB15 | [`xgboost_binary.ipynb`](notebooks/genids_unsw15/1.unsw_xgb_baseline_binary.ipynb) | [`isolation_forest_binary.ipynb`](notebooks/genids_unsw15/2.unsw_iforest_baseline_binary.ipynb) | [`xgboost_multiclass.ipynb`](notebooks/genids_unsw15/3.unsw_xgb_baseline_multiclass.ipynb) |
| GenIDS-CIC17 | [`xgboost_binary.ipynb`](notebooks/genids_cic17/1.cic17_xgb_baseline_binary.ipynb) | [`isolation_forest_binary.ipynb`](notebooks/genids_cic17/2.cic17_iforest_baseline_binary.ipynb) | [`xgboost_multiclass.ipynb`](notebooks/genids_cic17/3.cic17_xgb_baseline_multiclass.ipynb) |
| GenIDS-CIC18 | [`xgboost_binary.ipynb`](notebooks/genids_cic18/1.cic18_xgb_baseline_binary.ipynb) | [`isolation_forest_binary.ipynb`](notebooks/genids_cic18/2.cic18_iforest_baseline_binary.ipynb) | [`xgboost_multiclass.ipynb`](notebooks/genids_cic18/3.cic18_xgb_baseline_multiclass.ipynb) |

## Requirements

The notebooks were prepared for Python 3.8.10 and require the dependencies listed in the repository-level `requirements.txt` file.

From the repository root, create and activate a virtual environment and install the dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Start JupyterLab from the repository root:

```bash
jupyter lab
```

## Dataset Preparation

Download and extract the three processed datasets from the [GenIDS Benchmark](https://zenodo.org/records/21435638). The original public PCAP files are not required to execute these baseline notebooks.

A suggested local organization is:

```text
GenIDS-Framework/
|-- data/
|   |-- GenIDS-UNSW15_nfstream_binario.csv
|   |-- GenIDS-UNSW15_nfstream_multiclasse.csv
|   |-- GenIDS-CIC17_nfstream_binario.csv
|   |-- GenIDS-CIC17_nfstream_multiclasse.csv
|   |-- GenIDS-CIC18_nfstream_binario.csv
|   `-- GenIDS-CIC18_nfstream_multiclasse.csv
|-- baselines/
`-- requirements.txt
```

The datasets are distributed separately and should not be committed to this repository.

## Path Configuration

Each notebook contains a configuration cell near the beginning:

```python
from pathlib import Path

DATA_DIR = Path("../../../data").expanduser().resolve()
```

Update `DATA_DIR` if the datasets are stored elsewhere. An absolute path can be used, for example:

```python
DATA_DIR = Path("/path/to/GenIDS-Framework/data")
```

The `DATASET_FILES` mapping in the same cell can be updated when the downloaded files have different names. The notebook checks every required file before
loading the datasets and reports the missing paths when the configuration is incorrect.

## Common Preprocessing Pipeline

All baseline notebooks follow the same general preparation procedure:

1. load GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18;
2. inspect dataset dimensions and class distributions;
3. remove identifiers and metadata that are not part of the common feature space;
4. encode the application-related categorical attributes consistently across the datasets;
5. verify that the three datasets contain the same feature columns;
6. convert the features to numeric values and check for missing or infinite values;
7. split the source dataset into training and intradomain test partitions; and
8. fit `StandardScaler` on the source training partition and apply the fitted scaler to all evaluation datasets.

The preprocessing fitted for the source domain is reused without refitting in the interdomain tests.

## XGBoost Baselines

### Binary classification

The binary XGBoost notebooks use 20% of the source dataset for training and 80% for intradomain testing. The model is configured as follows:

```python
XGBClassifier(
    eval_metric="logloss",
    n_estimators=300,
    max_depth=10,
    objective="binary:logistic",
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    gamma=0.5,
    random_state=42,
    n_jobs=-1,
)
```

### Multiclass classification

The multiclass XGBoost notebooks use 30% of the source dataset for training and 70% for intradomain testing. The model uses three output classes and the following configuration:

```python
XGBClassifier(
    eval_metric="mlogloss",
    n_estimators=300,
    max_depth=10,
    objective="multi:softprob",
    num_class=3,
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    gamma=0.5,
    random_state=42,
    n_jobs=-1,
)
```

The notebooks also report the ten features with the highest XGBoost importance scores.

## Isolation Forest Baselines

The Isolation Forest notebooks implement no-supervised binary anomaly detection. Only normal flows from the source dataset are used for model training.

The normal source flows are divided as follows:

- 20% for model training; and
- 80% for validation.

The validation partition combines the unseen normal flows with all malicious flows from the source dataset. Youden's J statistic is computed from this partition to select the binary decision threshold. The same threshold is used without recalibration in both interdomain evaluations.

The Isolation Forest configuration is:

```python
IsolationForest(
    contamination="auto",
    n_estimators=200,
    max_samples=0.8,
    max_features=0.8,
    random_state=42,
    n_jobs=-1,
)
```

## Evaluation Metrics

The notebooks report the following metrics:

| Metric | Binary XGBoost | Multiclass XGBoost | Isolation Forest |
|---|:---:|:---:|:---:|
| Accuracy | Yes | Yes | Yes |
| Macro Precision | Yes | Yes | Yes |
| Macro Recall | Yes | Yes | Yes |
| Macro F1-score | Yes | Yes | Yes |
| AUC-ROC | Yes | Macro OvR | Yes |
| False Alarm Rate (FAR) | Yes | Per class and Macro | Yes |
| Average Precision | Yes | No | Yes |
| Confusion matrix | Yes | Yes | Yes |
| ROC curve | Yes | No | Yes |
| Precision–Recall curve | Yes | No | Yes |

For binary classification, FAR is computed as:

\[
\mathrm{FAR} = \frac{FP}{FP + TN}.
\]

For multiclass classification, each class is evaluated using a one-versus-rest confusion matrix. Macro FAR is the arithmetic mean of the class-specific FAR values.

## Running the Notebooks

The notebooks are independent and may be executed in any order. For each notebook:

1. configure `DATA_DIR` and, when necessary, `DATASET_FILES`;
2. select **Kernel > Restart Kernel and Run All Cells**;
3. confirm the displayed dataset dimensions and class distributions;
4. inspect the intradomain evaluation; and
5. inspect the two interdomain generalization evaluations.

The final cell produces a consolidated table containing the metrics for all three scenarios evaluated in that notebook.

## Expected Outputs

A successful execution produces:

- dataset and class-distribution summaries;
- training and evaluation partition sizes;
- the target-label mapping;
- feature-importance results for XGBoost;
- classification reports;
- confusion matrices;
- ROC and Precision–Recall curves for binary baselines;
- class-specific and Macro FAR for multiclass baselines; and
- a consolidated metrics table.

Numerical results should be reproducible when the same dataset versions, software dependencies, preprocessing configuration, and random seed are used. Execution time and memory consumption may vary according to the available hardware.

## Relationship to the Thesis Experiments

These notebooks provide the reference results used to compare the experimental interventions described in the doctoral thesis. They establish the performance of the models before applying PCA, Chi-Square feature selection, target-domain flow integration, or combinations of these interventions.

The baseline results should therefore be generated before comparing the gains or losses produced by the intervention notebooks.

## Data and Code Availability

- Processed datasets: [GenIDS Benchmark](https://zenodo.org/records/21435638)
- Framework repository: [GenIDS Framework](https://github.com/kelsonc/genids-framework)
- Dependency versions: repository-level [`requirements.txt`](../requirements.txt)

The source code is distributed under the license declared in the repository root. The processed datasets are distributed separately under the Creative Commons Attribution 4.0 International (CC BY 4.0) license.
