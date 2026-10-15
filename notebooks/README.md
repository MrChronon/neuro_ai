# Notebooks

Learning and research notebooks live here.

## 01 — MRI/fMRI spatial foundations

[`01_nifti_mri_fmri_basics.ipynb`](01_nifti_mri_fmri_basics.ipynb) uses real `sub-01` data from OpenNeuro `ds000117` to cover:

- NIfTI loading and 4D BOLD structure;
- voxel size, TR and field of view;
- T1w MRI vs BOLD fMRI / EPI;
- affine and voxel/world coordinate transforms;
- image orientation and canonical RAS;
- mean EPI construction;
- mutual-information-based EPI→T1w coregistration;
- translation and rigid transformations;
- resampling and visual QC.

The notebook is intentionally educational. It does not replace a full preprocessing workflow for inferential fMRI analysis.

Next notebook block: `events.tsv → HRF → convolution → design matrix → GLM`.
