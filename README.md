# QSAR-Modeling-and-Virtual-Screening-Pipeline

A full QSAR and virtual screening framework including data preparation, SMILES-to-SDF conversion, RDKit fingerprints, SHAP-based explainable models, and database screening. Designed for reproducible computational chemistry and drug discovery workflows.

It has been structured so that each step is modular, transparent, and easy to adapt to different datasets or project goals.

---

## Overview

This project provides a complete workflow for ligand-based drug discovery using Python and RDKit.  
The process begins with a raw IC₅₀/SMILES dataset, followed by data cleaning, fingerprint generation, QSAR model development, SHAP interpretation, and virtual screening of external chemical libraries.

All scripts follow consistent formatting, input/output conventions, and publication-quality plotting.

---

## The Repository Includes

- Data cleaning  
- Fingerprint generation  
- Fingerprint filtering  
- Batch QSAR modeling with automatic model selection  
- SHAP interpretability pipeline  
- Virtual screening of large molecular libraries  
- SMILES → SDF conversion for docking or MD workflows  

---

## Pipeline Structure

Below is a short description of each script and where it fits into the workflow.

### 2.1 **Data_cleaning.py**
Cleans ChEMBL-style bioactivity tables by removing missing values, enforcing nM units, generating pIC₅₀, applying an activity threshold, and removing duplicate SMILES.  
Optional canonicalization ensures consistent molecular identity across the dataset.

---

### 2.2 **generate_fingerprints.py**
Generates RDKit molecular fingerprints from SDF files using both modern and fallback RDKit APIs.  
Supports Morgan, MACCS, RDKit, AtomPair, and Torsion fingerprints.  
Outputs can be saved in CSV or Parquet format.

---

### 2.3 **filter_fingerprints.py**
Filters raw fingerprint matrices by removing constant bits, rare bits, highly correlated bits, and low-variance features.  
Metadata columns are preserved to maintain consistent identifiers throughout the workflow.

---

### 2.4 **qsar_modeling_shap.py**
Trains multiple QSAR models in parallel, evaluates their performance, tunes hyperparameters, and computes SHAP values for interpretation.  
Supports scikit-learn, XGBoost, LightGBM, Keras, and PyTorch-skorch.  
Outputs include organized directories containing models, plots, SHAP summaries, logs, and performance metrics.

---

### 2.5 **screen_database.py**
Uses a trained QSAR model to screen external chemical databases (CSV/Parquet).  
Ensures fingerprint alignment, generates prediction tables, highlights top-ranked molecules, and provides score distribution visualizations.

---

### 2.6 **smiles_to_sdf.py**
Converts SMILES into SDF files while preserving metadata as SD tags.  
Supports ETKDG conformer generation and MMFF/UFF optimization.  
Useful for docking workflows, MD simulations, or generating 3D descriptors.

---

## Environment Files

Two complete Conda environments are included:

- **QSAR_environment_cpu.yml** – for CPU-only setups  
- **QSAR_environment_gpu.yml** – for NVIDIA GPU-accelerated training  

Both environments include RDKit, scikit-learn, XGBoost, LightGBM, TensorFlow, PyTorch, SHAP, and all essential supporting libraries.

### Create the environment

conda env create -f QSAR_environment_cpu.yml  
# or  
conda env create -f QSAR_environment_gpu.yml  

### Activate the environment

conda activate QSAR_environment

---

## Workflow Summary

1. Clean your dataset  
python Data_cleaning.py --in raw.xlsx --out clean.xlsx --add-pic50 --active-threshold 6.0  

2. Convert SMILES to SDF  
python smiles_to_sdf.py --in clean.xlsx --out ligands.sdf  

3. Generate fingerprints  
python generate_fingerprints.py --in ligands.sdf --out fingerprints.csv  

4. Filter fingerprints  
python filter_fingerprints.py --in fingerprints.csv --out fingerprints_filtered.csv  

5. QSAR modeling with SHAP  
python qsar_modeling_shap.py --config config.json  

6. Screen external databases  
python screen_database.py --db zinc.csv --model models_out/  

---

## Citation and Copyright

This repository is copyright © 2025 **Muhammad Waqas**.  
All rights reserved.

The code is free to use **only for academic and research purposes**.  
Modification, redistribution, or commercial use is **not permitted**.

If you use any part of this pipeline, please cite:

Waqas, M. (2025). *QSAR Modeling and Virtual Screening Pipeline.*

---

## Contact

For questions, collaborations, or bug reports, please open an Issue or contact the author.
