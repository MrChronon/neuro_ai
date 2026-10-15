# MRI/fMRI spatial foundations: NIfTI, orientation and coregistration

This chapter records the first complete spatial neuroimaging checkpoint of the project using real data from **OpenNeuro ds000117 (Wakeman & Henson)**.

The corresponding code is in [`notebooks/01_nifti_mri_fmri_basics.ipynb`](../notebooks/01_nifti_mri_fmri_basics.ipynb).

![Spatial foundations summary](../figures/learning/spatial_foundations_summary.png)

## 1. Real data used

The practical work uses `sub-01`, MRI session `ses-mri`, task `facerecognition`, run 01.

The main functional file is:

```text
sub-01/ses-mri/func/
sub-01_ses-mri_task-facerecognition_run-01_bold.nii.gz
```

For the same run, BIDS also provides acquisition metadata (`*_bold.json`) and experimental timing (`*_events.tsv`). The latter will become the entry point to the next temporal-analysis block.

## 2. NIfTI and 4D fMRI

For run 01:

```text
shape = (64, 64, 33, 208)
voxel size / TR ≈ (3.0, 3.0, 3.75, 2.0)
```

Interpretation:

- `64 × 64 × 33` is the spatial grid of one 3D fMRI volume;
- `208` is the number of acquired volumes;
- spatial voxel size is approximately `3 × 3 × 3.75 mm`;
- `TR = 2 s`, so one whole-brain volume is acquired every two seconds;
- total run duration is approximately `416 s` (6 min 56 s).

A single voxel across time is a time series, while a single time point is a 3D brain volume.

## 3. Structural T1w MRI vs BOLD fMRI / EPI

The T1w image for the same participant has:

```text
shape = (192, 256, 256)
voxel size = (1.0, 1.0, 1.0) mm
```

One T1w voxel is `1 mm³`, while one fMRI voxel is approximately:

```text
3 × 3 × 3.75 = 33.75 mm³
```

These modalities serve different roles:

- **T1w MRI**: high-detail structural anatomy;
- **BOLD fMRI / EPI**: repeated lower-resolution measurements over time.

Resampling fMRI onto a 1 mm T1w grid does **not** create 1 mm functional information. It only represents the already measured EPI signal on a denser target grid through interpolation.

## 4. Affine: voxel coordinates and world coordinates

A voxel index such as `[30, 30, 15]` is only an address inside the array. The NIfTI affine maps this index to physical/world coordinates in millimetres.

For the real run:

```text
fMRI voxel: [30, 30, 15]
world coordinate ≈ [10.57, 7.89, 12.55] mm
T1w voxel coordinate ≈ [102.00, 100.31, 120.56]
nearest T1w voxel ≈ [102, 100, 121]
```

The key chain is:

```text
voxel index → affine → world coordinate → inverse affine of another image → voxel coordinate in that grid
```

This establishes geometric correspondence, but it does **not** prove that image contents are already perfectly aligned. That is the role of registration.

## 5. Orientation and canonical RAS

The two source images have different axis conventions:

```text
fMRI: LAS
T1w:  RAS
```

This explains why the same-looking array index can appear on a different side when raw arrays are plotted directly.

Using `nib.as_closest_canonical()` converts both images to a common canonical `RAS+` orientation for easier inspection.

Important distinction:

- **orientation**: anatomical direction of image axes;
- **canonicalization**: reordering/flipping axes into a conventional orientation;
- **registration**: estimating a transformation that aligns image content.

Therefore:

```text
canonical orientation ≠ registration
```

## 6. Field of view (FOV)

Approximate physical coverage was estimated as:

```text
number of voxels × voxel spacing
```

Result:

```text
fMRI FOV ≈ [192, 192, 123.75] mm
T1w FOV  ≈ [192, 256, 256] mm
```

This is a difference in **acquisition coverage**, not a difference in the biological size of the brain.

The fMRI acquisition covers a smaller physical volume, particularly along one spatial axis.

## 7. EPI-specific spatial limitations

EPI is fast enough for repeated whole-brain functional acquisition, but this speed makes it sensitive to magnetic-field inhomogeneity.

Near tissue-air boundaries, especially around the orbitofrontal cortex, eyes, and sinuses, EPI may show:

- **geometric distortion**: signal is displaced or locally compressed/stretched;
- **signal dropout**: signal is strongly attenuated or absent.

These are not the same as registration error.

A good rigid coregistration can align existing signal, but it cannot reconstruct signal that was never reliably measured.

## 8. Registration vs resampling

This checkpoint explicitly separated two operations:

- **registration** estimates a transformation between images;
- **resampling** applies that transformation and interpolates the moving image onto the target voxel grid.

In this notebook:

```text
static / fixed image = T1w
moving image         = mean EPI
```

The functional image is aligned to the participant's structural anatomy.

## 9. Mean EPI as the moving image

Instead of registering a single noisy fMRI volume, the 4D run is averaged across time:

```text
mean EPI(x, y, z) = mean over T of fMRI(x, y, z, t)
```

This produces one stable 3D functional image:

```text
shape = (64, 64, 33)
```

It is more suitable for spatial alignment than an arbitrary individual time point.

## 10. Mutual information for multimodal registration

T1w and EPI have different intensity contrasts, so direct voxel-wise intensity equality is not a useful objective.

Mutual information (MI) evaluates statistical dependence between the intensity distributions of the two modalities.

Conceptually:

```text
better anatomical alignment
→ more consistent cross-modal intensity relationship
→ higher mutual information
```

`nbins=32` in the DIPY metric is an analysis parameter used to discretize intensity values for estimating the joint distribution. It is not an MRI acquisition parameter.

## 11. Progressive coregistration

The registration was performed progressively:

```text
mean EPI
→ center-of-mass initialization
→ translation optimization (3 DOF)
→ rigid optimization (6 DOF)
→ resampling into T1w grid
```

### Translation-only result

The optimized translation matrix was approximately:

```text
[[ 1.      0.      0.     -0.248]
 [ 0.      1.      0.     -0.472]
 [ 0.      0.      1.      0.662]
 [ 0.      0.      0.      1.   ]]
```

This stage allows movement along `x`, `y`, and `z`, but no rotation.

### Rigid result

The final rigid matrix was approximately:

```text
[[ 0.99997   0.00026   0.00763   0.41950]
 [-0.00003   0.99954  -0.03046  -1.01654]
 [-0.00763   0.03046   0.99951   2.41858]
 [ 0.         0.         0.         1.     ]]
```

The non-zero off-diagonal values in the upper-left `3×3` block indicate a small rotation. Scaling and shear were not allowed.

This is appropriate for same-participant EPI↔T1w coregistration when the goal is to correct relative position and orientation rather than artificially reshape anatomy.

## 12. Visual quality control

Registration output must be inspected visually rather than accepted only because optimization converged.

### Translation vs rigid

![Translation versus rigid coregistration](../figures/results/coregistration_translation_vs_rigid.png)

Rigid alignment produced visibly better correspondence in the coronal and axial views while preserving good sagittal alignment.

### Contour QC

![Contour QC after rigid coregistration](../figures/results/coregistration_contour_qc.png)

Intensity contours from the registered mean EPI were overlaid on T1w anatomy in all three orthogonal planes.

The contour levels are only normalized EPI intensity levels. They should **not** be interpreted as anatomical tissue boundaries.

The remaining mismatch near the orbitofrontal region is consistent with the known vulnerability of EPI to susceptibility distortion and signal dropout and should not automatically be interpreted as failed rigid registration.

## 13. What this checkpoint established

The completed spatial chain is:

```text
BIDS
→ NIfTI
→ voxel / volume / TR
→ T1w vs BOLD
→ affine
→ voxel ↔ world coordinates
→ orientation / canonical RAS
→ FOV
→ EPI distortion and dropout
→ mean EPI
→ mutual information
→ coregistration
→ resampling
→ visual QC
```

Key distinctions:

- direct image geometry is not the same as anatomical registration;
- orientation is not registration;
- registration is not resampling;
- resampling does not increase the information content of fMRI;
- successful coregistration does not remove EPI distortion;
- missing EPI signal cannot be recovered by rigid registration.

## 14. Limitations of the educational pipeline

The notebook intentionally does **not** perform a full production fMRI preprocessing workflow.

Not yet included:

- motion correction / realignment of the 4D run;
- susceptibility distortion correction;
- brain extraction / masking;
- slice-timing correction;
- normalization to a standard space such as MNI;
- spatial smoothing;
- statistical GLM.

Future inferential analyses should use a standard preprocessing workflow (for example, fMRIPrep) while retaining the conceptual understanding developed here.

## Next step

The next block moves from space to time:

```text
events.tsv
→ temporal experimental structure
→ HRF
→ convolution
→ design matrix
→ GLM
```
