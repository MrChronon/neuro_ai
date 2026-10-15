# neuro_ai

A learning-to-research project in neuroimaging, computational cognitive science, computational neuroscience, and NeuroAI.

The project uses real multimodal neuroimaging data from the **Wakeman & Henson dataset (OpenNeuro ds000117)** and follows a staged path from experimental structure and data formats to fMRI analysis, EEG/MEG, machine learning, multimodal analysis, and NeuroAI.

## Current stage

**Stage 1: Neuroimaging foundations — spatial checkpoint completed**

Using real `sub-01` MRI/fMRI data, the project has now covered:

- BIDS structure and real NIfTI files;
- voxel, volume, 4D fMRI and TR;
- structural T1w MRI vs BOLD fMRI / EPI;
- affine matrices and `voxel ↔ world` coordinates;
- image orientation (`LAS`, `RAS`) and canonical orientation;
- field of view (FOV) and coverage;
- EPI-specific distortion and signal-dropout considerations;
- mean EPI construction;
- multimodal similarity with mutual information;
- center-of-mass initialization;
- translation-only registration (3 DOF);
- rigid EPI→T1w coregistration (6 DOF);
- resampling into the T1w grid;
- visual quality control in sagittal, coronal, and axial planes.

The spatial block is intentionally educational rather than a production preprocessing pipeline. Future statistical analyses will use a standard preprocessing workflow, while the notebook preserves the underlying concepts.

**Next learning block:**

`events.tsv → temporal experimental structure → HRF → convolution → design matrix → GLM`

## Visual overview

### How the real dataset is organized

![BIDS structure of ds000117](figures/learning/bids_structure_ds000117.png)

### Structural T1w MRI vs BOLD fMRI

![T1w MRI versus BOLD fMRI](figures/learning/t1w_vs_bold_fmri.png)

### Spatial foundations checkpoint

![Spatial foundations summary](figures/learning/spatial_foundations_summary.png)

### Coregistration quality control

![Translation versus rigid coregistration](figures/results/coregistration_translation_vs_rigid.png)

![Contour QC after rigid coregistration](figures/results/coregistration_contour_qc.png)

## Current learning materials

- [01 — MRI/fMRI spatial foundations: NIfTI, orientation and coregistration](docs/01_nifti_mri_fmri_basics.md)
- [01 — Real-data NIfTI / MRI / fMRI notebook](notebooks/01_nifti_mri_fmri_basics.ipynb)
- [Learning protocol](docs/LEARNING_PROTOCOL.md)

## Learning principle

Experiment → data → statistics → fMRI → ML → EEG/MEG → multimodal analysis → NeuroAI.

Complex methods are introduced only after the underlying data, assumptions, and measurement limitations are understood.

## Repository structure

- `notebooks/` — learning and research notebooks
- `src/` — reusable Python code
- `tests/` — tests for reusable code
- `configs/` — project configuration files
- `docs/` — learning chapters, project documentation, and methodological notes
- `figures/learning/` — conceptual learning figures
- `figures/results/` — visual results and QC figures derived from real data
- `results/` — derived numerical results
- `reports/` — research reports and drafts
- `references/` — notes and references to papers and datasets
- `data/` — local neuroimaging data; raw data are not committed to Git

## Language

The project is bilingual. Code, file names, variables, and technical identifiers are written in English. Explanatory materials may be available in Russian and English.

Russian version: [README_RU.md](README_RU.md)
