# BioMedical QA with Knowledge Graphs

**Course:** DA 331 — Big Data  
**Institution:** IIT Guwahati

**Authors:**
- Tanish Anand — 240150037
- Tumu Sai Seshi Kiran — 240150038

## 1. Project Overview

This project investigates whether augmenting biomedical question answering systems with structured medical knowledge graphs improves their performance.

We use the PubMedQA dataset and two biomedical knowledge graphs:

- **UMLS:** Unified Medical Language System
- **PrimeKG:** Precision Medicine Knowledge Graph

The project implements a pipeline consisting of:

1. Biomedical entity linking.
2. Knowledge graph construction and processing.
3. Fact retrieval from knowledge graphs.
4. Large language model-based question answering.
5. Classical machine learning experiments.
6. Evaluation and diagnostic analysis.

The experiments compare question-only and abstract-based baselines against systems augmented with relevant and irrelevant knowledge graph facts.

## 2. Repository Structure

The repository contains the following main files:

```text
BioMedical_QA_with_KG/
│
├── notebook/
│   └── biomedqa_kg.ipynb
│
├── .gitignore
├── README.md
├── report.pdf
├── contributions.pdf
└── BioMedical_QA_with_KG_Presentation_No_Notes.pptx
```

The datasets and generated experiment outputs are intentionally excluded from the repository.

## 3. Dataset Setup

Three primary data sources are required to reproduce the experiments.

### 3.1 PubMedQA

PubMedQA is a biomedical question-answering dataset containing questions, corresponding research abstracts and yes/no/maybe answers.

Download the dataset from the official repository:

https://github.com/pubmedqa/pubmedqa

The project uses the labelled PubMedQA dataset (PQA-L).

Required files:

```text
data/
├── ori_pqal.json
└── test_ground_truth.json
```

Download the corresponding files from the official PubMedQA repository and place them in the `data/` directory.

The official test set is kept separate from the development data. The experiments reported in this project use the development pool and do not use the official test set for model evaluation.

### 3.2 PrimeKG

PrimeKG is a biomedical knowledge graph connecting entities such as diseases, drugs, genes and phenotypes.

Official repository:

https://github.com/mims-harvard/PrimeKG

Dataset download:

https://dataverse.harvard.edu/api/access/datafile/6180620

Download the `kg.csv` file and place it in:

```text
data/
└── kg.csv
```

The notebook filters the original graph using the selected relation types before constructing the project-specific graph.

**Licensing note:** The PrimeKG software repository and the underlying dataset may have different licensing terms. Check the dataset's applicable terms before redistributing the downloaded data.

### 3.3 UMLS

UMLS is a collection of biomedical vocabularies and relationships maintained by the US National Library of Medicine (NLM).

Official download page:

https://www.nlm.nih.gov/research/umls/licensedcontent/umlsknowledgesources.html

The project uses the **UMLS 2026AB Metathesaurus Full Subset**.

Required files include:

- `MRCONSO.RRF`
- `MRREL.RRF`
- `MRSTY.RRF`
- `MRSAB.RRF`

#### Obtaining UMLS

1. Create a UMLS Terminology Services (UTS) account.
2. Review and accept the UMLS Metathesaurus License.
3. Submit the licence request.
4. Wait for NLM approval.
5. Sign in and download the required UMLS release.
6. Extract the downloaded files.

UTS registration and licence information:

https://uts.nlm.nih.gov/license

**Important licensing restrictions**

UMLS is licensed data. Access is granted to individuals under the applicable licence terms.

Do not upload or redistribute:
- Original UMLS download archives.
- Raw UMLS source files.
- Derived UMLS graph data.
- Verbatim UMLS-derived fact text.
- Other restricted UMLS content.

These restrictions also apply to public GitHub repositories. A private repository does not automatically grant permission to redistribute licensed data.

Each user intending to reproduce the UMLS experiments must obtain their own appropriate access and comply with the licence. This was stated in the UMLS guidelines.

### Why UMLS Data and Derived Results Are Not Included

This project uses the UMLS Metathesaurus as one of its biomedical knowledge sources. UMLS is a licensed resource provided by the U.S. National Library of Medicine (NLM). Access requires registration and acceptance of the applicable licence terms.

For this reason, the repository does not include the downloaded UMLS files or raw UMLS-derived data, such as extracted concept relationships, synonym mappings, or retrieved fact text. We have kept these materials out of the public repository to avoid redistributing licensed content without confirming that such redistribution is permitted by the applicable licence.

The exclusion of UMLS-derived materials does **not** mean that the UMLS experiments were not conducted. UMLS was used in the knowledge graph construction, retrieval and evaluation stages of the project. The corresponding experimental results are discussed in the project report and presentation.

Where appropriate, the repository may include aggregate evaluation metrics, comparison tables and figures that do not expose restricted UMLS content. Users wishing to reproduce the UMLS experiments must obtain the resource independently from the official NLM source and comply with its licence requirements.

**Official source:** [UMLS Knowledge Sources](https://www.nlm.nih.gov/research/umls/licensedcontent/umlsknowledgesources.html)

## 4. Local Environment Setup

### Requirements

The project uses Python and Jupyter Notebook, together with libraries for data processing, machine learning, knowledge graph construction and language model inference.

A machine with sufficient memory and disk space is recommended, particularly for UMLS extraction and language model experiments.

The notebook uses PySpark for processing the large UMLS source files.

### Step 1: Clone the repository

```bash
git clone https://github.com/tanish028/BioMedicalQA_with_KG.git
cd BioMedicalQA_with_KG
```

### Step 2: Create a Python environment

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

On Linux or macOS:

```bash
source venv/bin/activate
```

### Step 3: Install Jupyter Notebook

```bash
pip install jupyter notebook
```

The notebook contains its own dependency setup section. Run the initial setup and installation cells before proceeding with the experiments.

### Step 4: Prepare the data directory

Create the following directory structure:

```text
BioMedical_QA_with_KG/
│
├── data/
│   ├── ori_pqal.json
│   ├── test_ground_truth.json
│   ├── kg.csv
│   └── umls-2026AB-metathesaurus-full/
│       └── 2026AB/
│           └── META/
│               ├── MRCONSO.RRF
│               ├── MRREL.RRF
│               ├── MRSTY.RRF
│               └── MRSAB.RRF
│
└── notebook/
    └── biomedqa_kg.ipynb
```

The UMLS directory structure should match the paths configured in the notebook.

If the downloaded files are stored elsewhere, update the corresponding input paths in the notebook configuration.

### Step 5: Run the notebook

Start Jupyter:

```bash
jupyter notebook
```

Open:

```text
notebook/biomedqa_kg.ipynb
```

Run the notebook from the beginning, following the setup, input validation and execution cells.

The notebook performs the following stages:

1. Environment setup and input validation.
2. PubMedQA loading and dataset preparation.
3. PrimeKG filtering and UMLS extraction using Spark.
4. Knowledge graph construction.
5. Biomedical entity linking.
6. Relevant and irrelevant fact retrieval.
7. LLM inference and evaluation.
8. Classical machine learning experiments.
9. Diagnostic analyses and result consolidation.

The notebook saves intermediate checkpoints for long-running stages. These checkpoints allow interrupted experiments to resume, provided the saved configuration matches the current experiment settings.

## 5. Running on Google Colab

Google Colab is an alternative execution environment, particularly for GPU-based language model experiments.

### Step 1: Open the notebook

Open the repository on GitHub and navigate to:

`notebook/biomedqa_kg.ipynb`

Download the notebook or use Colab's GitHub import option to open it.

### Step 2: Configure the runtime

In Colab:

1. Open Runtime.
2. Select Change runtime type.
3. Choose a GPU runtime.
4. Save the settings.

A Tesla T4 GPU was used for the language model experiments in this project.

GPU availability depends on the Colab account and current resource allocation.

### Step 3: Mount Google Drive

The notebook includes a Google Drive setup option.

Mount Drive when prompted and upload the required datasets to your Drive.

Maintain the expected directory structure and update the configured paths if necessary.

### Step 4: Restore or upload input files

Use the notebook's input upload/restore section to provide the required datasets.

Ensure that the following files are accessible:

- PubMedQA input files.
- PrimeKG `kg.csv`.
- Required UMLS Metathesaurus files.

UMLS must be obtained independently under the appropriate licence.

### Step 5: Execute the notebook

Run the notebook cells sequentially.

The notebook includes input validation and checkpoint functionality. Long-running model inference and data processing stages can resume from compatible saved checkpoints.

**Note:** Running the entire notebook from scratch can take considerable time and requires substantial storage and compute resources.

## 6. Outputs

The notebook automatically creates a `results/` directory.

It contains outputs organised into different stages of the experiment, including:

- Dataset summaries.
- Knowledge graph statistics.
- Retrieved facts and retrieval summaries.
- LLM predictions and evaluation metrics.
- Classical ML cross-validation results.
- Diagnostic analyses.
- Figures and visualisations.
- Consolidated experiment summaries.

The results ZIP generated by the notebook is an output archive, not a required input for reproducing the project.

Generated results are excluded from this GitHub repository.

## 7. Reproducibility Notes

For meaningful reproduction:

- Use the same dataset versions and preprocessing settings.
- Keep the development and official test sets separate.
- Maintain the configured random seeds.
- Use compatible model versions and inference settings.
- Do not mix checkpoints from different configurations.
- Obtain licensed datasets independently.
- Record any changes to paths, model settings or dataset versions.

Results may vary depending on hardware, library versions and model execution environments.

## 8. Citation and Acknowledgements

### PubMedQA

Jin, Q., Dhingra, B., Liu, Z., Cohen, W., and Lu, X. (2019).

*PubMedQA: A Dataset for Biomedical Research Question Answering.*

EMNLP-IJCNLP 2019.

### PrimeKG

Chandak, P., Huang, K., and Zitnik, M. (2023).

*Building a knowledge graph to enable precision medicine.*

Scientific Data.

https://doi.org/10.1038/s41597-023-01960-3

### UMLS

National Library of Medicine (NLM).

Unified Medical Language System (UMLS).

https://www.nlm.nih.gov/research/umls/

---

**Project:** BioMedical QA with Knowledge Graphs  
**Course:** DA 331 — Big Data  
**Institution:** IIT Guwahati
