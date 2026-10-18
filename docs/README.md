# Learning documentation

This directory contains structured learning chapters and methodological notes produced during the NeuroAI research track.

The chapters follow the same real-data workflow used in the notebooks. Raw neuroimaging data are not stored in the repository.

## Chapters

1. [MRI/fMRI spatial foundations: NIfTI, orientation and coregistration](01_nifti_mri_fmri_basics.md)
   - NIfTI and 4D BOLD data
   - voxel, volume, TR and field of view
   - T1w MRI vs BOLD fMRI / EPI
   - affine and voxel/world coordinates
   - orientation and canonical RAS
   - EPI distortion and signal dropout
   - mean EPI
   - mutual information
   - translation and rigid coregistration
   - resampling and three-plane visual QC

2. [Experimental events, HRF and convolution: learning recap](02_events_hrf_convolution.md)
   - `events.tsv` and 9 stimulus conditions
   - canonical HRF and overlapping responses
   - discrete convolution and `np.convolve`
   - 93 real onsets → modeled predictor
   - `faces` (61) vs `scrambled` (32), verified linearity
   - methodological assumptions and next checkpoint

## Project protocol

- [Learning protocol](LEARNING_PROTOCOL.md)

## Current checkpoint

Spatial foundations are complete at the introductory practical level. The temporal events → HRF → convolution learning checkpoint is also completed (no GLM or measured BOLD effects yet).

## Next step

`event durations → fMRI acquisition time sampling (208 volumes) → design matrix → GLM`
