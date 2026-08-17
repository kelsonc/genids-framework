# Network-Flow Extraction

This directory contains the Jupyter notebooks used to extract bidirectional network flows from the original PCAP captures of UNSW-NB15, CIC-IDS2017, and CIC-IDS2018 using NFStream. It also contains the dataset-specific notebooks that consolidate the extracted flow files before the subsequent preprocessing and customization stage.

The original PCAP captures and the generated CSV files are not stored in this GitHub repository. Users must obtain the original datasets from their official sources and configure the local input and output paths in the first configuration cell of each notebook.

## Scope

The notebooks in this component perform the following operations:

- validation of the locally stored PCAP files;
- bidirectional network-flow extraction with NFStream;
- removal of auxiliary columns and incomplete records;
- conversion of NFStream timestamps to the timezone used by the original capture;
- application of dataset-specific daily labeling rules, when required;
- removal of duplicate flow records;
- export of intermediate daily or capture-group CSV files;
- consolidation of the intermediate files into one dataset-specific flow file.

Feature selection, encoding, scaling, construction of experimental scenarios, baseline training, and data interventions are outside the scope of this directory. Those operations belong to the subsequent components of GenIDS Framework.

## Directory Structure

```text
flow_extraction/
├── README.md
├── features.pdf
└── notebooks/
    ├── genids_unsw15/
    │   ├── 1_extraction_daily_flows_unsw.ipynb
    │   └── 2_full_flows_unsw.ipynb
    ├── genids_cic17/
    │   ├── 1_extraction_flows_cic17_monday.ipynb
    │   ├── 2_extraction_flows_cic17_tuesday.ipynb
    │   ├── 3_extraction_flows_cic17_wednesday.ipynb
    │   ├── 4_extraction_flows_cic17_thursday.ipynb
    │   ├── 5_extraction_flows_cic17_friday.ipynb
    │   └── 6_full_flows_cic17.ipynb
    └── genids_cic18/
        ├── 1_extraction_flows_cic18_tuesday.ipynb
        ├── 2_extraction_flows_cic18_wednesday.ipynb
        ├── 3_extraction_flows_cic18_wednesday.ipynb
        ├── 4_extraction_flows_cic18_wednesday.ipynb
        ├── 5_extraction_flows_cic18_thursday.ipynb
        ├── 6_extraction_flows_cic18_thursday.ipynb
        ├── 7_extraction_flows_cic18_thursday.ipynb
        ├── 8_extraction_flows_cic18_friday.ipynb
        ├── 9_extraction_flows_cic18_friday.ipynb
        ├── 10_extraction_flows_cic18_friday.ipynb
        └── 11_full_flows_cic18.ipynb
```

The generated CSV files should be stored outside the source-notebook directories. A suggested local organization is:

```text
local_data/
├── original_pcaps/
│   ├── unsw_nb15/
│   ├── cic_ids2017/
│   └── cic_ids2018/
└── extracted_flows/
    ├── genids_unsw15/
    ├── genids_cic17/
    └── genids_cic18/
```

This local directory is only a recommendation. Any location can be used by changing the `Path` variables in the configuration cell of each notebook.

## General NFStream Configuration

The extraction notebooks use the following common configuration:

| Parameter | Value |
|---|---:|
| `idle_timeout` | 300 seconds |
| `active_timeout` | 20 seconds |
| `statistical_analysis` | `True` |
| `decode_tunnels` | `True` |
| `bpf_filter` | `ip` |

The complete description of the NFStream attributes is available in [`features.pdf`](features.pdf).

## Input Data

The original PCAP captures must be obtained from the official dataset sources:

- [UNSW-NB15](https://research.unsw.edu.au/projects/unsw-nb15-dataset);
- [CIC-IDS2017](https://www.unb.ca/cic/datasets/ids-2017.html);
- [CIC-IDS2018](https://www.unb.ca/cic/datasets/ids-2018.html).

The notebooks do not download or redistribute the original captures. Before execution, users must:

1. obtain the required PCAP files from the official source;
2. preserve the filenames expected by the corresponding notebook or update the filename list in its configuration cell;
3. set the PCAP input directory;
4. set the directory where the extracted CSV files will be written.

## UNSW-NB15 Workflow

| Order | Notebook | Responsibility | Main output |
|---:|---|---|---|
| 1 | `1_extraction_daily_flows_unsw.ipynb` | Extract flows from the January and February PCAP groups. | `pcap1.csv`, `pcap2.csv` |
| 2 | `2_full_flows_unsw.ipynb` | Consolidate the extracted groups, associate the UNSW-NB15 reference labels, standardize the labels, reduce the benign class, and export the complete flow file. | `GenIDS-NB15.csv` |

The first notebook processes 53 January PCAP files and 26 sequentially named February PCAP files. The second notebook uses the original UNSW-NB15 flow table to associate the extracted flows with their attack categories.

## CIC-IDS2017 Workflow

| Order | Notebook | Main labels | Output |
|---:|---|---|---|
| 1 | `1_extraction_flows_cic17_monday.ipynb` | Benign | `01_nfstream_monday.csv` |
| 2 | `2_extraction_flows_cic17_tuesday.ipynb` | FTP-Patator, SSH-Patator | `02_nfstream_tuesday.csv` |
| 3 | `3_extraction_flows_cic17_wednesday.ipynb` | DoS variants, Heartbleed | `03_nfstream_wednesday.csv` |
| 4 | `4_extraction_flows_cic17_thursday.ipynb` | Web attacks, Infiltration | `04_nfstream_thursday.csv` |
| 5 | `5_extraction_flows_cic17_friday.ipynb` | DDoS, Botnet, PortScan | `05_nfstream_friday.csv` |
| 6 | `6_full_flows_cic17.ipynb` | Consolidation and GenIDS class mapping | `GenIDS-CIC17.csv` |

Notebooks 01–05 extract, clean, and label the flows associated with each daily PCAP capture. Notebook 06 validates and concatenates the five daily CSV files, maps the attack labels to the GenIDS representation, and exports the complete dataset-specific flow file.

## CIC-IDS2018 Workflow

| Order | Notebook | Main attack type | Output |
|---:|---|---|---|
| 1 | `1_extraction_flows_cic2018_tuesday.ipynb` | DDoS LOIC | `01_cic2018_tuesday_nfstream_20_02_18.csv` |
| 2 | `2_extraction_flows_cic2018_wednesday.ipynb` | FTP/SSH Brute Force | `02_cic2018_wednesday_nfstream_14_02_18.csv` |
| 3 | `3_extraction_flows_cic2018_wednesday.ipynb` | DDoS HOIC/LOIC | `03_cic2018_wednesday_nfstream_ddos_21_02_18.csv` |
| 4 | `4_extraction_flows_cic2018_wednesday.ipynb` | Infiltration | `04_cic2018_wednesday_nfstream_28_02_18.csv` |
| 5 | `5_extraction_flows_cic2018_thursday.ipynb` | DoS GoldenEye/Slowloris | `05_cic2018_thursday_nfstream_dos_15_02_18.csv` |
| 6 | `6_extraction_flows_cic2018_thursday.ipynb` | Web attacks | `06_cic2018_thursday_nfstream_webattack_22_02_18.csv` |
| 7 | `7_extraction_flows_cic2018_thursday.ipynb` | Infiltration | `07_cic2018_thursday_nfstream_infiltration_01_03_18.csv` |
| 8 | `8_extraction_flows_cic2018_friday.ipynb` | DoS Hulk/SlowHTTPTest | `08_cic2018_friday_nfstream_dos_16_02_18.csv` |
| 9 | `9_extraction_flows_cic2018_friday.ipynb` | Web attacks | `09_cic2018_friday_nfstream_webattack_23_02_18.csv` |
| 10 | `10_nfstream_cic2018_friday.ipynb` | Botnet | `10_cic2018_friday_nfstream_botnet_02_03_18.csv` |
| 11 | `11_full_flows_cic18.ipynb` | Consolidation and GenIDS class mapping | `GenIDS-CIC18.csv` |

Notebooks 01–10 validate and consolidate all PCAP parts configured for their corresponding date and attack type. Notebook 11 receives only the ten generated CSV files; it does not repeat the NFStream extraction.

## Label Organization

The daily notebooks retain the detailed attack labels required by the original labeling procedure. During dataset-specific consolidation, labels are organized into the GenIDS representation:

| Final label | Description |
|---|---|
| `benign` | Legitimate network flows. |
| `ddos` | DoS and DDoS flows selected for the evaluated attack class. |
| `background` | Other malicious-flow categories retained as background attacks. |

The binary label distinguishes `benign` from `malign` flows. The subsequent dataset-customization stage is responsible for applying the final common feature space and the remaining preprocessing operations required by the experiments.

## Execution Order

Run the notebooks inside each dataset directory in numerical order. Each extraction notebook must complete successfully before its corresponding consolidation notebook is executed.

For each notebook:

1. open the first configuration cell;
2. set the local PCAP or CSV input path;
3. set the output directory;
4. verify the configured filenames;
5. select the project Python kernel;
6. execute all cells from first to last;
7. confirm that the final cell reports the expected output path and dataset shape.

## Expected Validation Behavior

The notebooks stop with an explicit error when:

- an input directory does not exist;
- a required PCAP or CSV file is missing;
- an expected label or timestamp column is absent;
- daily CSV schemas are incompatible during consolidation;
- unlabeled flows remain after application of the daily rules; or
- the output file cannot be created.

The notebooks are delivered without stored execution outputs. This keeps the source artifacts compact and prevents results produced under one local configuration from being interpreted as reference outputs.

## Software Requirements

The extraction workflow requires:

- Python 3.8 or newer;
- Jupyter Notebook or JupyterLab;
- NFStream;
- Pandas;
- Pytz.

The fixed package versions used by GenIDS Framework must be installed from the repository-level `requirements.txt`.

## Generated Artifacts

The files produced by this component are intermediate research artifacts. The standardized datasets distributed through GenIDS Benchmark are generated only after the subsequent dataset-customization stage. Therefore, locally generated CSV files should not be considered identical to the published Benchmark files until all documented preprocessing and standardization steps have been completed.
