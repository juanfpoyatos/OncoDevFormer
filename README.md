# OncoDevFormer

**OncoDevFormer** is a representation-learning framework designed to compare developmental epithelial variation with malignant cell-state variation in pancreatic cancer. The project uses single-cell RNA-seq data from normal pancreatic development and pancreatic cancer to ask a central question: **do malignant epithelial states recapitulate developmental trajectories, or do they occupy distinct transcriptional directions?**

The analysis combines developmental reference modeling, BERT-style embedding approaches, geometric comparisons in learned expression spaces, and biological annotation using curated gene programs. Rather than treating cancer states as simple reactivations of development, **DevCancer** separates two related but different concepts: where malignant cells lie relative to developmental cell states, and whether malignant transitions actually follow developmental axes.

---

### Overview

**OncoDevFormer** investigates the relationship between pancreatic development and pancreatic cancer using single-cell transcriptomic data.

The project builds a developmental reference space from normal epithelial populations and then projects malignant epithelial states into that space. This makes it possible to quantify how much of cancer variation can be explained by developmental programs and how much lies outside them.

The main goals are to:

- assemble and preprocess developmental and cancer single-cell datasets;
- train or apply a BERT-style language model to single-cell gene-expression representations;
- extract gene and cell embeddings from the model;
- define developmental epithelial axes, including foregut-to-ductal, foregut-to-acinar/tip, and foregut-to-biliary directions;
- compare malignant-state transitions with developmental directions;
- distinguish developmental positioning from malignant-state variation;
- annotate learned axes and residual cancer structure using curated biological programs.

Together, the analyses test whether pancreatic cancer progression is well described as movement along developmental trajectories or whether malignant cells exploit broader and partly non-developmental transcriptional programs.

---

### Data

The project uses single-cell RNA-seq datasets representing two biological contexts:

1. **Normal pancreatic development**  
   Developmental epithelial populations are used to construct a reference space capturing major axes of normal pancreas and foregut-related variation.

2. **Pancreatic cancer**  
   Malignant epithelial cells are organized into cancer-associated states, including normal-like ductal states, transitional Kras-mutant/metaplastic states, and malignant ductal states.

The developmental and cancer datasets are harmonized through preprocessing, gene matching, normalization, and embedding-based representation. Particular care is taken to avoid over-interpreting coarse labels such as “ductal,” since some cancer-associated ductal-like populations may reflect mixed epithelial, biliary-like, metaplastic, or malignant programs rather than a pure developmental ductal identity.

---

### Approach

The analysis proceeds in four broad stages.

First, developmental and cancer single-cell datasets are assembled and preprocessed into comparable gene-expression spaces. The preprocessing steps include filtering, normalization, gene-symbol harmonization, and preparation of input matrices for downstream modeling.

Second, a BERT-style model is applied to single-cell expression profiles. This produces learned representations of cells and genes that can be used to compare developmental and malignant transcriptional states.

Third, developmental reference axes are defined from normal epithelial populations. Cancer cells and cancer-state centroids are then projected into this developmental coordinate system. This allows the project to quantify whether malignant states are positioned near developmental identities and whether transitions between cancer states align with developmental lineage directions.

Fourth, the geometric results are interpreted biologically using marker genes and curated transcriptional programs. These include epithelial identity, ductal/biliary markers, acinar markers, endocrine markers, EMT/invasion, KRAS-MAPK signaling, MYC activity, p53 response, proliferation, DNA damage, hypoxia, glycolysis, oxidative phosphorylation, inflammatory signaling, antigen presentation, senescence, apoptosis, stemness, and therapy-resistance-associated programs.

---

### Repository Structure

#### Main Notebooks

- `Data_assembly.ipynb`  
  Initial data assembly. Loads the developmental and cancer single-cell datasets, organizes metadata, checks gene identifiers, and prepares the starting objects for downstream analysis.

- `Data_assembly2.ipynb`  
  Data preprocessing and harmonization. Performs filtering, normalization, gene matching, dataset alignment, and construction of the processed matrices used in the modeling and comparison steps.

- `Bert.ipynb`  
  Runs the BERT-style language model workflow. Generates learned cell and/or gene embeddings from single-cell expression representations and prepares model-derived outputs for downstream geometric analyses.

- `Results_section1.ipynb`  
  First results section. Defines the developmental reference space and examines how developmental epithelial populations are organized in the learned representation.

- `Results_section2.ipynb`  
  Second results section. Projects cancer states into the developmental framework and compares malignant-state transitions with developmental axes. This notebook contains the core geometric analyses separating developmental position from malignant variation.

- `Results_section2_Biological_Annotation.ipynb`  
  Biological annotation of the geometric results. Uses marker genes and transcriptional programs to interpret developmental axes, cancer residual structure, and state-specific malignant features.

- `Results_MousePrograms.ipynb`  
  Mouse cancer-program analysis. Scores curated mouse transcriptional programs across cancer states to annotate recurrent tumor-intrinsic and stress-response programs.

---

### Key Analyses

DevCancer includes several linked analyses:

- construction of a developmental epithelial reference space;
- comparison of developmental and cancer expression distributions;
- projection of malignant cells into developmental PC or embedding spaces;
- decomposition of cancer variation into developmental and non-developmental components;
- quantification of how much malignant displacement is captured by canonical developmental lineage axes;
- comparison of cancer transition vectors with developmental directions;
- testing whether transitional malignant states are simple intermediates between ductal-like and malignant ductal endpoints;
- marker-based validation of ductal, acinar, endocrine, biliary, and hepatocyte-like signals;
- scoring of curated cancer-associated transcriptional programs;
- interpretation of residual malignant variation using EMT, KRAS, inflammatory, metabolic, proliferative, and stress-response programs.

---

### Biological Motivation

Epithelial cancers are often described as aberrant reactivations of developmental programs. While this idea is biologically plausible, it can also become too simplistic. A malignant cell may resemble a developmental state in one coordinate system while still varying along directions that are not part of normal developmental progression.

DevCancer addresses this distinction directly. The project asks whether pancreatic cancer states merely occupy positions along developmental lineages or whether they generate structured malignant variation outside the developmental reference space.

The results are intended to support a more careful interpretation: development provides an informative coordinate system for describing cancer states, but it does not necessarily explain the dominant axes of malignant progression.

---

### Setup

This project uses Python-based single-cell and machine-learning workflows. Core dependencies include common scientific and single-cell analysis libraries such as:

- `numpy`
- `pandas`
- `scipy`
- `scikit-learn`
- `scanpy`
- `anndata`
- `matplotlib`
- `seaborn`
- `torch`
- `transformers`

Some workflows may depend on additional packages used for BERT-style model training, embedding extraction, and gene-program scoring. See `requirements.txt` for the recommended environment.

To install dependencies:

```bash
pip install -r requirements.txt
```

---

### Outputs

The notebooks generate:

- processed developmental and cancer expression matrices;
- harmonized met:adata tables;
- BERT-derived cell and gene embeddings;
- developmental PCA/reference coordinates;
- cancer-state centroid comparisons;
- developmental-axis projection scores;
- cosine similarity analyses between developmental and malignant transitions;
- biological program scores;
- figures used in the main results and supplementary analyses.

---

### Citation

If you use this repository or adapt parts of the analysis, please cite the associated manuscript or preprint once available.

---

### Project Status

This repository is under active development. File names, figure outputs, and analysis notebooks may change as the project is refined.
