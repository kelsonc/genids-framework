# Data-Intervention Experiments

## Overview

This directory contains the notebooks used to evaluate the data interventions investigated in the doctoral thesis **Generalization of Machine Learning-Based Intrusion Detection Systems**.

The experiments use the standardized datasets distributed through the [GenIDS Benchmark](https://zenodo.org/records/21435638):

- **GenIDS-NB15**, derived from UNSW-NB15;
- **GenIDS-CIC17**, derived from CIC-IDS2017; and
- **GenIDS-CIC18**, derived from CIC-IDS2018.

The directory contains 21 notebooks. Interventions IN1 and IN2 independently evaluate PCA and Chi-Square, respectively; IN3 evaluates flow integration; and IN4 and IN5 evaluate flow integration combined with PCA and Chi-Square, respectively.

## Interventions

| Intervention | Description | Notebooks |
|---|---|---:|
| IN1 | PCA-based dimension reduction | 6 |
| IN2 | Chi-Square feature selection | 6 |
| IN3 | Integration of labeled target-domain flows | 3 |
| IN4 | Target-domain flow integration followed by PCA | 3 |
| IN5 | Target-domain flow integration followed by Chi-Square | 3 |

## Directory Structure

```text
experiments/
|-- README.md
`-- notebooks/
    |-- in1/
    |   |-- 1.unsw_xgb_pca.ipynb
    |   |-- 2.cic17_xgb_pca.ipynb
    |   |-- 3.cic18_xgb_pca.ipynb
    |   |-- 4.unsw_iforest_pca.ipynb
    |   |-- 5.cic17_iforest_pca.ipynb
    |   `-- 6.cic18_iforest_pca.ipynb
    |
    |-- in2/
    |   |-- 1.unsw_xgb_chisquare.ipynb
    |   |-- 2.cic17_xgb_chisquare.ipynb
    |   |-- 3.cic18_xgb_chisquare.ipynb
    |   |-- 4.unsw_iforest_chisquare.ipynb
    |   |-- 5.cic17_iforest_chisquare.ipynb
    |   `-- 6.cic18_iforest_chisquare.ipynb
    |
    |-- in3/
    |   |-- experiment_in3_1.ipynb
    |   |-- experiment_in3_2.ipynb
    |   `-- experiment_in3_3.ipynb
    |
    |-- in4/
    |   |-- experiment_in4_1.ipynb
    |   |-- experiment_in4_2.ipynb
    |   `-- experiment_in4_3.ipynb
    |
    `-- in5/
        |-- experiment_in5_1.ipynb
        |-- experiment_in5_2.ipynb
        `-- experiment_in5_3.ipynb
```

## Requirements

The notebooks were prepared for Python 3.8.10. Install the fixed dependencies from the repository root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab
```

The hardware configuration reported in the root README describes the authors' execution environment and is not a strict minimum requirement. Execution time and memory consumption depend on the dataset, intervention, and integration rate.

## Dataset Preparation

Download and extract GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18 from the [GenIDS Benchmark](https://zenodo.org/records/21435638). The original public PCAP files are not required to execute these experiments.

The notebooks contain a `DATA_DIR` variable near the beginning. Update this variable to point to the local dataset directory before running a notebook:

```python
from pathlib import Path

DATA_DIR = Path("/path/to/genids/datasets")
```

If the downloaded files use different names, update the `DATASET_FILES` mapping or dataset-path variables in the experiment configuration cell. Dataset files must not be committed to this repository.

## Intervention IN1: PCA-Based Dimension Reduction

IN1 evaluates Principal Component Analysis as an isolated intervention. PCA transforms the common space of 70 flow features into 25 principal components.

The scaler and PCA transformation are fitted on source-domain training data and are reused without refitting in the intradomain and interdomain evaluations.

| Notebook | Source domain | Algorithm | Classification |
|---|---|---|---|
| [`1.unsw_xgb_pca.ipynb`](notebooks/in1/1.unsw_xgb_pca.ipynb) | GenIDS-NB15 | XGBoost | Binary |
| [`2.cic17_xgb_pca.ipynb`](notebooks/in1/2.cic17_xgb_pca.ipynb) | GenIDS-CIC17 | XGBoost | Binary |
| [`3.cic18_xgb_pca.ipynb`](notebooks/in1/3.cic18_xgb_pca.ipynb) | GenIDS-CIC18 | XGBoost | Binary |
| [`4.unsw_iforest_pca.ipynb`](notebooks/in1/4.unsw_iforest_pca.ipynb) | GenIDS-NB15 | Isolation Forest | Binary anomaly detection |
| [`5.cic17_iforest_pca.ipynb`](notebooks/in1/5.cic17_iforest_pca.ipynb) | GenIDS-CIC17 | Isolation Forest | Binary anomaly detection |
| [`6.cic18_iforest_pca.ipynb`](notebooks/in1/6.cic18_iforest_pca.ipynb) | GenIDS-CIC18 | Isolation Forest | Binary anomaly detection |

The XGBoost notebooks use 20% of the source data for training and 80% for intradomain testing. The Isolation Forest notebooks train on 20% of the normal source flows and combine the remaining normal flows with all source attacks for validation. The Isolation Forest decision threshold is selected with Youden's J statistic and reused without recalibration in both interdomain tests.

Each notebook reports the explained-variance ratios, cumulative explained variance, PCA loadings, and evaluation results.

## Intervention IN2: Chi-Square Feature Selection

IN2 evaluates Chi-Square as an isolated feature-selection intervention. The method selects 25 features from the common 70-feature space.

| Notebook | Source domain | Algorithm | Classification |
|---|---|---|---|
| [`1.unsw_xgb_chisquare.ipynb`](notebooks/in2/1.unsw_xgb_chisquare.ipynb) | GenIDS-NB15 | XGBoost | Binary |
| [`2.cic17_xgb_chisquare.ipynb`](notebooks/in2/2.cic17_xgb_chisquare.ipynb) | GenIDS-CIC17 | XGBoost | Binary |
| [`3.cic18_xgb_chisquare.ipynb`](notebooks/in2/3.cic18_xgb_chisquare.ipynb) | GenIDS-CIC18 | XGBoost | Binary |
| [`4.unsw_iforest_chisquare.ipynb`](notebooks/in2/4.unsw_iforest_chisquare.ipynb) | GenIDS-NB15 | Isolation Forest | Binary anomaly detection |
| [`5.cic17_iforest_chisquare.ipynb`](notebooks/in2/5.cic17_iforest_chisquare.ipynb) | GenIDS-CIC17 | Isolation Forest | Binary anomaly detection |
| [`6.cic18_iforest_chisquare.ipynb`](notebooks/in2/6.cic18_iforest_chisquare.ipynb) | GenIDS-CIC18 | Isolation Forest | Binary anomaly detection |

The XGBoost notebooks fit `MinMaxScaler` and `SelectKBest(chi2, k=25)` on the source training partition. For Isolation Forest, the training partition contains only normal flows and therefore cannot support supervised Chi-Square selection. Following the original experiment, its selector is fitted on the labeled source-domain validation partition. No target-domain labels are used for feature selection or threshold calibration.

Each notebook reports the selected attributes, their Chi-Square scores, and the intradomain and interdomain results.

## Intervention IN3: Network Flow Integration

IN3 evaluates the integration of labeled flows from the target domain into the source-domain training data. Three integration strategies are considered:

- benign flows;
- malicious (D)DoS flows; and
- mixed benign and malicious (D)DoS flows.

| Notebook | Strategy | Integration direction | Default rate |
|---|---|---|---:|
| [`experiment_in3_1.ipynb`](notebooks/in3/experiment_in3_1.ipynb) | Benign | GenIDS-CIC18 → GenIDS-NB15 | 20% |
| [`experiment_in3_2.ipynb`](notebooks/in3/experiment_in3_2.ipynb) | Malicious (D)DoS | GenIDS-CIC18 → GenIDS-CIC17 | 20% |
| [`experiment_in3_3.ipynb`](notebooks/in3/experiment_in3_3.ipynb) | Mixed | GenIDS-CIC18 → GenIDS-NB15 | 20% |

The notebooks can reproduce the 20%, 40%, 60%, and 80% scenarios by changing `INTEGRATION_RATE` and rerunning all cells.

The selected target-domain flows are removed from the target test partition to prevent direct overlap between training and evaluation. An equivalent number of source-domain flows is removed when required to control the final training set size.

## Intervention IN4: PCA and Network Flow Integration

IN4 combines labeled target-domain flow integration with PCA. PCA reduces the 70-feature space to 25 principal components and is fitted on the resulting training data after flow integration.

| Notebook | Strategy | Integration direction | Reported default rate |
|---|---|---|---:|
| [`experiment_in4_1.ipynb`](notebooks/in4/experiment_in4_1.ipynb) | Benign | GenIDS-CIC18 → GenIDS-CIC17 | 40% |
| [`experiment_in4_2.ipynb`](notebooks/in4/experiment_in4_2.ipynb) | Malicious (D)DoS | GenIDS-CIC17 → GenIDS-CIC18 | 60% |
| [`experiment_in4_3.ipynb`](notebooks/in4/experiment_in4_3.ipynb) | Mixed | GenIDS-CIC17 → GenIDS-CIC18 | 20% |

Where indicated in the configuration cell, the integration-rate variable may be changed to reproduce the remaining 20%, 40%, 60%, and 80% scenarios.

## Intervention IN5: Chi-Square and Network Flow Integration

IN5 combines labeled target-domain flow integration with Chi-Square feature selection. `SelectKBest` selects 25 features and is fitted on the training partition after flow integration.

| Notebook | Strategy | Integration direction | Reported default rate |
|---|---|---|---:|
| [`experiment_in5_1.ipynb`](notebooks/in5/experiment_in5_1.ipynb) | Benign | GenIDS-CIC18 → GenIDS-CIC17 | 40% |
| [`experiment_in5_2.ipynb`](notebooks/in5/experiment_in5_2.ipynb) | Malicious (D)DoS | GenIDS-CIC17 → GenIDS-CIC18 | 60% |
| [`experiment_in5_3.ipynb`](notebooks/in5/experiment_in5_3.ipynb) | Mixed | GenIDS-CIC17 → GenIDS-CIC18 | 20% |

The integration-rate variable may be changed to reproduce the 20%, 40%, 60%, and 80% scenarios.

## Common Reproducibility Controls

The notebooks preserve the principal controls adopted in the experiments:

- `random_state=42` for stochastic operations;
- source-domain preprocessing reused in interdomain evaluation;
- removal of integrated target flows from the target test partition;
- controlled training-set size in flow-integration experiments;
- PCA fitted on training data;
- Chi-Square fitted on the designated source-domain partition; and
- fixed XGBoost and Isolation Forest hyperparameters.

The notebooks for IN3, IN4, and IN5 use XGBoost. The conclusions obtained from these nine experiments should therefore be interpreted within the evaluated algorithm and dataset configurations.

## Evaluation Outputs

Depending on the intervention and classification setting, a successful execution produces:

- dataset dimensions and class distributions;
- selected-flow counts and remaining target test samples;
- PCA explained-variance diagnostics;
- selected Chi-Square features and scores;
- classification reports and confusion matrices;
- Accuracy, Macro Precision, Macro Recall, and Macro F1-score;
- AUC-ROC;
- False Alarm Rate (FAR);
- Average Precision and Precision–Recall curves for binary experiments; and
- consolidated result tables.

For binary experiments, FAR is computed as:

\[
\mathrm{FAR} = \frac{FP}{FP + TN}.
\]

For multiclass experiments, FAR is computed for each class using a one-versus-rest confusion matrix and then averaged to obtain Macro FAR.

## Running the Experiments

The notebooks are independent and can be executed individually. For each notebook:

1. configure `DATA_DIR` and the expected dataset filenames;
2. review the source domain, target domain, class values, and integration rate;
3. select **Kernel > Restart Kernel and Run All Cells**;
4. confirm the displayed dataset and partition summaries;
5. inspect the intradomain and interdomain results; and
6. record the configuration together with the final metrics table.

For IN3–IN5, change only the documented integration-rate variable when reproducing another percentage. Restart the kernel and execute all cells after each change to avoid retaining state from a previous scenario.

## Relationship to the Baselines

The notebooks in [`../baselines/`](../baselines/) provide the reference results without the interventions evaluated here. IN1–IN5 should be compared against the corresponding source domain, algorithm, classification setting, and evaluation domain in the baseline results.

## Data and Code Availability

- Processed datasets: [GenIDS Benchmark](https://zenodo.org/records/21435638)
- Framework repository: [GenIDS Framework](https://github.com/kelsonc/genids-framework)
- Dependency versions: repository-level [`requirements.txt`](../requirements.txt)
- Reproducibility instructions: [`docs/REPRODUCIBILITY.md`](../docs/REPRODUCIBILITY.md)

The source code is distributed under the license declared in the repository root. The processed datasets are distributed separately under the Creative Commons Attribution 4.0 International (CC BY 4.0) license.
