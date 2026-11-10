# OncoDevFormer

**OncoDevFormer** is a representation-learning framework designed to compare developmental epithelial variation with malignant cell-state variation in pancreatic cancer. The project uses single-cell RNA-seq data from normal pancreatic development and pancreatic cancer to ask a central question: **do malignant epithelial states recapitulate developmental trajectories, or do they occupy distinct transcriptional directions?**

The analysis combines developmental reference modeling, BERT-style embedding approaches, geometric comparisons in learned expression spaces, and biological annotation using curated gene programs. Rather than treating cancer states as simple reactivations of development, **OncoDevFormer** separates two related but different concepts: where malignant cells lie relative to developmental cell states, and whether malignant transitions actually follow developmental axes.

---

### Overview

**OncoDevFormer** investigates the relationship between pancreatic development and pancreatic cancer using single-cell transcriptomic data.

The project builds a developmental reference space from normal epithelial populations and then projects malignant epithelial states into that space. This makes it possible to quantify how much of cancer variation can be explained by developmental programs and how much lies outside them.

The main goals are to:

- assemble and preprocess developmental and cancer single-cell datasets;
- train a BERT-style language model to single-cell gene-expression representations;
- extract classification-token (CLS) cell embeddings from the model;
- define developmental epithelial axes, including foregut-to-ductal, foregut-to-acinar, and foregut-to-biliary directions;
- compare malignant-state transitions with developmental directions;
- distinguish developmental positioning from malignant-state variation;
- annotate learned axes and residual cancer structure using curated biological programs.

Together, the analyses test whether pancreatic cancer progression is well described as movement along developmental trajectories or whether malignant cells exploit broader and partly non-developmental transcriptional programs.

---

### Data

The project uses single-cell RNA-seq datasets representing two biological contexts:

1. **Normal pancreatic development**  
   Developmental epithelial populations were used to construct a reference capturing the major axes of normal pancreatic and foregut variation.
   This single‑cell timelapse of mouse prenatal development —from gastrula to birth— was produced by Jay Shendure’s lab, Department of Genome Sciences, University of Washington. Description of the project is available [here](https://omg.gs.washington.edu/jax/public/about.html), and the dataset used [here](https://shendure-web.gs.washington.edu/content/members/cxqiu/public/backup/jax/download/adata/).

2. **Pancreatic cancer**  
   Malignant epithelial cells are organized into cancer-associated states, including normal-like ductal states, transitional Kras-mutant/metaplastic states, and malignant ductal states.
   This dataset was produced  by Scott Lowe's lab, Memorial Sloan Kettering Cancer Center, New York. Description of the project is available [here](https://www.mskcc.org/research/ski/labs/scott-lowe/accelerating-discovery-using-mouse-models), and the specific dataset used (GSE207938_ProgressionCohort.h5ad) [here](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE207938).

The developmental and cancer datasets are harmonized through preprocessing, gene matching, normalization, and embedding-based representation. Particular care is taken to avoid over-interpreting coarse labels such as “ductal,” since some cancer-associated ductal-like populations may reflect mixed epithelial, biliary-like, metaplastic, or malignant programs rather than a pure developmental ductal identity.

---

### Approach

The analysis proceeds in four broad stages.

First, developmental and cancer single-cell datasets are assembled and preprocessed into comparable gene-expression spaces. The preprocessing steps include filtering, normalization, gene-symbol harmonization, and preparation of input matrices for downstream modeling.

Second, a BERT-style model is applied to single-cell expression profiles. This produces learned representations of cells that can be used to compare developmental and malignant transcriptional states.

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


#### Additional experiments and robustness analyses (AE notebooks; Supplementary Table S3)

The original notebooks above document the initial OncoDevFormer analysis. The **additional experiments (AE)** were developed for the manuscript revision to revise that workflow, generate the definitive representations, and examine whether its biological conclusions are robust to data selection, model training, reference definitions, and geometric assumptions. These analyses are described in the supplementary material in connection with **Table S3**. They complement rather than replace the descriptions of the original notebooks above.

The revised notebooks are listed below in analysis order. `AE1b` establishes the common audited inputs and representations for the downstream experiments; individual AE notebooks then examine distinct potential sources of uncertainty. The notebook descriptions here are based on their documented aims and analyses, not a claim that every generated diagnostic is included in the paper.

| Additional experiment | Notebook | Main question and analyses |
| --- | --- | --- |
| **AE1 — Audit of the original computational pipeline** | [`AE1.ipynb`](notebooks/AE1.ipynb) | Reconstructs the original developmental and pancreatic-cancer cell selection, epithelial-filtering decisions, gene harmonization, MLM input preparation, and masking procedure. Identifies issues requiring correction before the definitive analyses, while documenting the provenance of earlier results. |
| **AE1b — Audited model reconstruction** | [`AE1b.ipynb`](notebooks/AE1b.ipynb) | Rebuilds the corrected developmental corpus and aligned gene matrices; uses a common tokenizer and corrected MLM masking to train matched **joint** (development + cancer) and **development-only** encoders across seeds. Saves the authoritative manifest, cell-order checks, and embeddings used downstream. |
| **AE2 — Developmental subspace and residual-structure sensitivity** | [`AE2.ipynb`](notebooks/AE2.ipynb) | Reassesses projection of cancer states into developmental PCA subspaces across numbers of components; examines reconstruction distributions, appropriate null/benchmark comparisons, residual organization, and biological-sample-aware uncertainty in the audited joint representation. |
| **AE3 — Biological definition and validation of developmental directions** | [`AE3.ipynb`](notebooks/AE3.ipynb) | Revisits the Qiu developmental labels and their temporal coverage, validates pancreatic lineage-related reference states with markers, and recomputes early/late/total malignant displacement alignment with revised developmental directions. Includes stage-aware weighting, sample-aware summaries, and checks of malignant-state positioning. |
| **AE4 — Joint versus development-only representation learning** | [`AE4.ipynb`](notebooks/AE4.ipynb) | Compares matched joint and development-only MLM encoders, holding preprocessing and developmental biological definitions fixed. Tests whether projection, directional alignment, and malignant residual structure depend on having included cancer cells during representation training. |
| **AE5 — Independent normal-pancreas calibration** | [`AE5.ipynb`](notebooks/AE5.ipynb) | Uses held-out developmental cells and an independent adult-normal pancreas reference from **Tabula Muris** to calibrate developmental projections and residuals. Assesses adult acinar, ductal, and endocrine populations separately and checks the sensitivity of external queries to model/domain-token choices. |
| **AE6 — Comparison with alternative representations** | [`AE6.ipynb`](notebooks/AE6.ipynb) | Compares the audited MLM with conventional normalized-expression PCA and **scVI** representations. Repeats developmental projection, malignant displacement, and residual analyses, including comparisons matched by the amount of developmental variance represented rather than assuming that equal PC counts are directly comparable. |
| **AE7 — Local developmental geometry** | [`AE7.ipynb`](notebooks/AE7.ipynb) | Tests whether apparently development-unexplained malignant variation could result from curvature of the developmental manifold rather than a distinct direction. Uses local developmental tangent spaces, held-out developmental and adult-normal calibration, matched-dimensional random benchmarks, and neighborhood/dimensionality sensitivity checks. |

**Analysis sequence.** Start with the historical notebooks for context and **AE1** for the revision; use **AE1b** to create the definitive aligned data, paired encoders, and manifest. **AE2–AE3** reconstruct the primary geometric and biological analyses. **AE4–AE7** test training-composition, normal-reference, representation, and manifold-geometry dependencies. Several downstream analyses reuse the developmental split and adult-normal query set established in AE5; consult each notebook's path and provenance checks before running it.

**Interpretive scope.** Developmental *proximity* of malignant cells and developmental *alignment* of changes between malignant states are distinct quantities. Projection fractions depend on the learned representation and the dimensions of the reference, whereas the directional and biological comparisons are used to assess which conclusions persist across controls. The AE series is intended to document these distinctions, not to assert that a geometric residual is automatically cancer-specific.

#### Revised manuscript analyses and figure-production notebooks

The following notebooks assemble or interpret outputs from the revised pipeline and are distinct from the AE controls:

- [`Edited_Results_1_developmental_PC_annotation.ipynb`](notebooks/Edited_Results_1_developmental_PC_annotation.ipynb) — Biological annotation of the leading developmental principal components, including independent-seed checks.
- [`Edited_Results_4_malignant_state_programs.ipynb`](notebooks/Edited_Results_4_malignant_state_programs.ipynb) — Developmental and cancer-program scoring across malignant states, with biological-sample-aware summaries and interpretation of residual axes.
- [`Edited_main_figure_panels_v5.ipynb`](notebooks/Edited_main_figure_panels_v5.ipynb) — Recreates computational panels used in the revised main figures from the audited outputs; final figure layout can be assembled separately.

**File naming.** The supplementary analyses are designated **AE1, AE1b, and AE2–AE7** in the revised manuscript and repository. Some notebook headings, code paths, and cached output filenames may still carry their earlier `WP` labels; those internal provenance/output identifiers should not be changed without updating the corresponding dependencies. The GitHub notebook filenames listed above assume the revised files have been renamed `AE*_FINAL.ipynb` before upload.

---

### Key Analyses
OncoDevFormer includes several linked analyses:

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

OncoDevFormer addresses this distinction directly. The project asks whether pancreatic cancer states merely occupy positions along developmental lineages or whether they generate structured malignant variation outside the developmental reference space.

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
