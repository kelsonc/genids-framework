# Code Documentation

This document describes the organization and main responsibilities of the code associated with the doctoral thesis **"Generalization of Machine Learning-Based Intrusion Detection Systems"**.

## Repository Components

The GenIDS Framework is organized into three main code components:

1. `flow_extraction/`: network-flow extraction and dataset customization;
2. `baselines/`: reference models without the proposed interventions; and
3. `experiments/`: implementations of interventions IN1-IN5.

The baseline and experiment directories contain 30 independent Jupyter notebooks: nine baselines and 21 intervention experiments. Each notebook keeps its configuration parameters near the beginning and can be inspected or executed without importing code from another notebook.

## Flow Extraction and Dataset Customization

The notebooks under `flow_extraction/notebooks/` use NFStream to extract bidirectional network flows from the original PCAP files of UNSW-NB15, CIC-IDS2017, and CIC-IDS2018. Their main responsibilities include:

- reading the original PCAP files;
- extracting statistical, packet-size, temporal, protocol, and application attributes;
- cleaning and consolidating daily flow files;
- assigning binary and multiclass labels;
- selecting a common 70-feature representation; and
- producing the customized GenIDS-NB15, GenIDS-CIC17, and GenIDS-CIC18 datasets.

The processed datasets are distributed separately through the [GenIDS Benchmark](https://doi.org/10.5281/zenodo.21435638). Detailed extraction instructions are provided in [`flow_extraction/README.md`](../flow_extraction/README.md).

## Baseline Notebooks

The nine notebooks under `baselines/notebooks/` establish the reference results used to assess the interventions. Each source dataset is evaluated with binary XGBoost, binary Isolation Forest, and multiclass XGBoost.

| Source domain | Binary XGBoost | Binary Isolation Forest | Multiclass XGBoost |
|---|---|---|---|
| GenIDS-NB15 | `baselines/notebooks/genids_nb15/xgboost_binary.ipynb` | `baselines/notebooks/genids_nb15/isolation_forest_binary.ipynb` | `baselines/notebooks/genids_nb15/xgboost_multiclass.ipynb` |
| GenIDS-CIC17 | `baselines/notebooks/genids_cic17/xgboost_binary.ipynb` | `baselines/notebooks/genids_cic17/isolation_forest_binary.ipynb` | `baselines/notebooks/genids_cic17/xgboost_multiclass.ipynb` |
| GenIDS-CIC18 | `baselines/notebooks/genids_cic18/xgboost_binary.ipynb` | `baselines/notebooks/genids_cic18/isolation_forest_binary.ipynb` | `baselines/notebooks/genids_cic18/xgboost_multiclass.ipynb` |

Each notebook trains on one source domain, performs an intradomain hold-out evaluation, and evaluates the same fitted model and preprocessing objects on the other two domains. Isolation Forest is trained only with normal source flows and uses a labeled source-domain validation partition to calibrate its decision threshold.

See [`baselines/README.md`](../baselines/README.md) for the data partitions, model hyperparameters, metrics, and execution instructions.

## Experiment Notebooks

The 21 notebooks under `experiments/notebooks/` implement five interventions.

| Intervention | Method | Models | Notebooks |
|---|---|---|---:|
| IN1 | PCA applied independently | XGBoost and Isolation Forest | 6 |
| IN2 | Chi-Square applied independently | XGBoost and Isolation Forest | 6 |
| IN3 | Labeled target-domain flow integration | XGBoost | 3 |
| IN4 | Flow integration combined with PCA | XGBoost | 3 |
| IN5 | Flow integration combined with Chi-Square | XGBoost | 3 |

### IN1: PCA-based dimension reduction

The six notebooks in `experiments/notebooks/in1/` apply PCA as an isolated intervention. The source-domain feature space is transformed from 70 original features to 25 principal components. The fitted scaler and PCA transformation are reused without refitting in interdomain evaluations.

### IN2: Chi-Square feature selection

The six notebooks in `experiments/notebooks/in2/` select 25 of the 70 common features. XGBoost experiments fit the Min-Max scaler and selector on the source training partition. For Isolation Forest, the selector is fitted on the labeled source-domain validation partition because the model-training partition contains only normal flows.

### IN3-IN5: target-domain flow integration

The nine notebooks in `experiments/notebooks/in3/`, `in4/`, and `in5/` evaluate the integration of labeled target-domain flows. The strategies integrate benign, malicious (D)DoS, or mixed flows. IN4 applies PCA after integration, whereas IN5 applies Chi-Square feature selection after integration.

Selected target-domain flows are removed from the corresponding target test partition to prevent direct overlap between training and evaluation.

See [`experiments/README.md`](../experiments/README.md) for the complete notebook index, integration directions, default rates, and methodological details.

## Main Function Groups

Although the notebooks are self-contained, their helper functions follow common conceptual groups.

### Data loading and preprocessing

These functions load CSV files, remove identifiers and metadata, encode categorical columns consistently, standardize numeric types, verify the shared feature representation, and create source-domain partitions.

### Feature interventions

PCA experiments fit `StandardScaler` and `PCA`. Chi-Square experiments fit `MinMaxScaler` followed by `SelectKBest(score_func=chi2, k=25)`, ensuring that the selector receives non-negative input values.

### Flow selection and integration

The IN3-IN5 helpers select target-domain flows by class and timestamp, remove selected records from the target test set, integrate them into the training set, and control the final training-set size according to the experiment configuration.

### Model construction and evaluation

Model helpers construct XGBoost or Isolation Forest estimators and report the metrics used in the thesis. Evaluation helpers generate classification reports, confusion matrices, ROC and Precision-Recall curves where applicable, feature importance, PCA diagnostics, Chi-Square scores, and consolidated result tables.

## Metrics

Depending on the classification setting, the notebooks report Accuracy, Macro Precision, Macro Recall, Macro F1-score, Attack Recall, AUC-ROC, Average Precision, and False Alarm Rate (FAR). For multiclass tasks, FAR is computed one-versus-rest for each class, and Macro FAR is the arithmetic mean of the class-specific values.

## Traceability to the Thesis

| Thesis evidence | Responsible artifacts |
|---|---|
| Reference performance without interventions | `baselines/notebooks/` |
| Isolated effect of PCA | `experiments/notebooks/in1/` |
| Isolated effect of Chi-Square | `experiments/notebooks/in2/` |
| Effect of target-domain flow integration | `experiments/notebooks/in3/` |
| Effect of integration combined with PCA | `experiments/notebooks/in4/` |
| Effect of integration combined with Chi-Square | `experiments/notebooks/in5/` |
| Construction of the standardized datasets | `flow_extraction/notebooks/` |

This mapping allows a reviewer to move from a methodological component or experimental conclusion in the thesis to the directory responsible for its implementation.

## Maintainability Notes

- Configuration parameters, dataset paths, integration rates, and random seeds are kept near the beginning of each notebook.
- `RANDOM_STATE = 42` is used where stochastic behavior must be controlled.
- Preprocessing objects fitted on source-domain data are reused without refitting on target-domain test data.
- Each notebook is intentionally self-contained to support independent inspection and execution.
- Generated outputs should be stored separately from source notebooks.

