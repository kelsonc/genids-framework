# Reproducibility Guide

This document describes how to reproduce the baselines and intervention experiments associated with the doctoral thesis **"Generalization of Machine Learning-Based Intrusion Detection Systems"**. It complements the repository README files with a unified execution workflow.

## 1. Reproduction Scope

The principal experimental artifact contains 30 notebooks:

- nine baseline notebooks;
- six notebooks for IN1 (PCA);
- six notebooks for IN2 (Chi-Square); and
- nine notebooks for IN3-IN5 (flow integration and its combinations with PCA and Chi-Square).

The flow-extraction notebooks provide an additional, optional path for reconstructing the GenIDS datasets from the original PCAP files. They are not required when the processed datasets are downloaded from the GenIDS Benchmark.

## 2. Reference Environment

The experiments reported in the associated studies were executed in the following reference environment:

| Component | Configuration |
|---|---|
| Operating system | Ubuntu 20.04.6 LTS (64-bit), Linux kernel 5.4.0-216-generic |
| Processor | Intel(R) Xeon(R) E-2224G CPU @ 3.50 GHz |
| Memory | 32 GB DDR4 RAM |
| Storage | 2 TB (1 TB SATA SSD + 1 TB HDD) |
| Python | 3.8.10 |
| Development environment | Jupyter Notebook or JupyterLab |

This configuration is a reference rather than a strict minimum. Execution on other hardware is possible, but memory use and runtime can vary. Computational cost was not measured as an experimental variable; therefore, no fixed runtime estimate is provided.

## 3. Software Installation

Clone the repository and create a Python environment from its root directory:

```bash
git clone https://github.com/kelsonc/genids-framework.git
cd genids-framework
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab
```

Use the package versions declared in `requirements.txt` when numerical comparability with the reference environment is required.

## 4. Dataset Preparation

Download and extract the processed datasets from the permanent [GenIDS Benchmark record](https://doi.org/10.5281/zenodo.21435638):

- GenIDS-NB15;
- GenIDS-CIC17; and
- GenIDS-CIC18.

The original PCAP files are not required for baseline or intervention execution. Keep the extracted CSV files outside version control. A suggested organization is:

```text
genids-framework/
|-- data/
|-- baselines/
|-- experiments/
|-- flow_extraction/
`-- requirements.txt
```

The notebooks contain a `DATA_DIR` variable near the beginning. Update it to the directory containing the extracted CSV files:

```python
from pathlib import Path

DATA_DIR = Path("/absolute/path/to/genids/datasets")
```

If an extracted filename differs from the filename expected by a notebook, change only the corresponding entry in `DATASET_FILES` or the dataset-path variable in the configuration cell.

## 5. Recommended Execution Order

The notebooks are independent, but the following order makes comparison with the thesis easier:

1. execute the nine baselines;
2. execute IN1 and compare its results with the corresponding baselines;
3. execute IN2 and compare its results with the corresponding baselines;
4. execute IN3 for benign, malicious, and mixed integration;
5. execute IN4 for the same three integration strategies; and
6. execute IN5 for the same three integration strategies.

For each notebook:

1. configure `DATA_DIR` and the expected filenames;
2. restart the kernel;
3. run all cells from first to last;
4. verify dataset dimensions, feature columns, and class distributions; and
5. retain the consolidated metrics table and diagnostic outputs.

## 6. Baseline Reproduction

The baseline notebooks are located under `baselines/notebooks/` and are indexed in [`baselines/README.md`](../baselines/README.md).

Each notebook uses one dataset as the source domain. The model is evaluated on an intradomain hold-out partition and then on the other two datasets without refitting the preprocessing objects or model.

The baseline matrix contains:

- binary XGBoost for each of the three source domains;
- multiclass XGBoost for each of the three source domains; and
- binary no-supervised Isolation Forest for each source domain.

Successful baseline execution produces the reference metrics used to calculate or interpret gains and losses introduced by IN1-IN5.

## 7. IN1 and IN2 Reproduction

IN1 and IN2 are located under `experiments/notebooks/in1/` and `experiments/notebooks/in2/`, respectively.

### IN1: PCA

- Classification: binary.
- Models: XGBoost and Isolation Forest.
- Source domains: GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18.
- Transformation: 70 original features to 25 principal components.
- Leakage control: the scaler and PCA are fitted on source-domain data and reused without refitting on interdomain test data.

### IN2: Chi-Square

- Classification: binary.
- Models: XGBoost and Isolation Forest.
- Source domains: GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18.
- Selection: 25 features from the common 70-feature space.
- XGBoost fitting rule: Min-Max scaling and Chi-Square selection are fitted on the source training partition.
- Isolation Forest fitting rule: the supervised selector is fitted on the labeled source-domain validation partition because the training partition contains only normal flows.

No target-domain labels are used to fit IN1 or IN2 transformations.

## 8. IN3-IN5 Reproduction

The nine notebooks under `experiments/notebooks/in3/`, `in4/`, and `in5/` use XGBoost and evaluate three target-domain integration strategies:

- benign-flow integration;
- malicious (D)DoS-flow integration; and
- mixed benign and malicious (D)DoS-flow integration.

The configuration cell of each notebook identifies its source domain, target domain, strategy, and default integration rate. Where supported, set `INTEGRATION_RATE` to `0.20`, `0.40`, `0.60`, or `0.80` and rerun the entire notebook to reproduce the evaluated proportions.

IN3 applies flow integration alone. IN4 applies PCA after integration, and IN5 applies Chi-Square feature selection after integration. Selected target-domain flows must remain excluded from the target test partition.

The complete notebook mapping and default configurations are documented in [`experiments/README.md`](../experiments/README.md).

## 9. Expected Outputs and Metrics

Depending on the notebook, successful execution produces:

- dataset dimensions and class distributions;
- source training, intradomain test, and interdomain test sizes;
- selected or integrated flow counts;
- PCA explained variance and loadings;
- selected Chi-Square features and scores;
- classification reports and confusion matrices;
- ROC and Precision-Recall curves where applicable;
- feature-importance plots; and
- consolidated evaluation tables.

The metrics include Accuracy, Macro Precision, Macro Recall, Macro F1-score, Attack Recall, AUC-ROC, Average Precision, and FAR, as applicable. Binary FAR is computed as `FP / (FP + TN)`. Multiclass FAR is computed one-versus-rest for each class, and Macro FAR is their arithmetic mean.

Notebook outputs may use ratios in the interval `[0, 1]`, whereas thesis tables may present the same values on a percentage scale.

## 10. Reproducibility Controls

The following conditions must be preserved:

1. use the same GenIDS dataset versions;
2. keep `RANDOM_STATE = 42` where defined;
3. preserve the notebook's source and target domains;
4. preserve the binary or multiclass label mapping;
5. fit preprocessing and feature interventions only on the partition specified by the notebook;
6. exclude integrated target-domain flows from target testing;
7. preserve model hyperparameters and integration rates; and
8. execute every notebook from a restarted kernel in top-to-bottom order.

Small numerical differences may occur because of platform-specific behavior in numerical libraries. Large differences should first be investigated by checking the dataset version, filename mapping, class labels, source/target direction, dependency versions, and integration rate.

## 11. Successful Reproduction Checklist

A reproduction is considered complete when:

- the selected notebooks execute from first to last cell without exceptions;
- the displayed datasets, source/target directions, labels, and rates match the intended configuration;
- the expected tables and diagnostic outputs are produced;
- no target test observation used for integration remains in the target test partition; and
- the reproduced results support the same qualitative behavior reported for the corresponding baseline or intervention.

For a complete thesis-level reproduction, execute all nine baselines and all 21 intervention notebooks. For a focused reproduction, select the baseline and intervention notebooks associated with the specific claim being assessed.

