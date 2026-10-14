# NIfTI, fMRI and structural MRI: first practical checkpoint

This chapter records the first hands-on neuroimaging milestone of the project using real data from **OpenNeuro ds000117 (Wakeman & Henson)**.

The code is available in [`notebooks/01_nifti_mri_fmri_basics.ipynb`](../notebooks/01_nifti_mri_fmri_basics.ipynb).

## 1. NIfTI in this project

NIfTI (`.nii` / `.nii.gz`) is the image format used to store neuroimaging arrays together with spatial information.

For the first BOLD run of `sub-01`:

```text
shape = (64, 64, 33, 208)
voxel size / TR = (3.0, 3.0, 3.75, 2.0)
```

Interpretation:

- `64 × 64 × 33` is the spatial voxel grid of one fMRI volume;
- `208` is the number of volumes in time;
- each voxel is approximately `3 × 3 × 3.75 mm`;
- a new volume is acquired every `2 s` (`TR = 2 s`);
- the run spans approximately `208 × 2 = 416 s`, or 6 min 56 s.

A 4D fMRI array can be thought of as:

```text
X × Y × Z × T
```

Examples:

```python
data[:, :, :, 0]
```

returns one complete 3D volume at the first time point.

```python
data[30, 30, 15, :]
```

returns the time series of one voxel across all 208 volumes.

## 2. Visualizing a 3D fMRI volume

A 3D volume is inspected through 2D orthogonal slices:

- sagittal: fix `X`;
- coronal: fix `Y`;
- axial: fix `Z`.

For example:

```python
first_volume[:, :, z]
```

fixes one `Z` index and therefore returns a 2D array.

The fMRI image looks relatively coarse because its voxels are large compared with structural MRI. This is part of the acquisition trade-off between spatial resolution, temporal resolution, signal quality, and brain coverage.

## 3. Structural T1w MRI vs BOLD fMRI

For the same participant, the T1-weighted anatomical image has:

```text
shape = (192, 256, 256)
voxel size = (1.0, 1.0, 1.0) mm
```

This is a 3D image with much finer spatial sampling.

One T1w voxel:

```text
1 × 1 × 1 = 1 mm³
```

One fMRI voxel:

```text
3 × 3 × 3.75 = 33.75 mm³
```

So one fMRI voxel in this acquisition is about **33.75 times larger by volume** than one T1w voxel.

The roles of the two images are different:

```text
T1w MRI
→ high-detail anatomy

BOLD fMRI
→ repeated 3D measurements used to study BOLD signal changes over time
```

BOLD remains an **indirect** measure related to neural activity through neurovascular coupling.

## 4. Different voxel grids

The central voxel indices of T1w and fMRI are not automatically the same physical point.

For example:

```text
T1w grid:  192 × 256 × 256, 1 mm voxels
fMRI grid:  64 × 64 × 33, 3 × 3 × 3.75 mm voxels
```

The two images describe the same participant but use different grids, resolutions, orientations, and origins.

This motivates the next spatial concept: the **affine matrix**.

## 5. Affine: voxel coordinates to world coordinates

A voxel index such as:

```text
[30, 30, 15]
```

is an address inside a NumPy array. It is not yet a physical anatomical coordinate.

A NIfTI affine is a `4 × 4` transformation that maps voxel coordinates:

```text
[i, j, k]
```

to physical/world coordinates:

```text
[x, y, z] mm
```

The observed affine matrices for `sub-01` differ between the BOLD and T1w images.

The T1w affine is close to an axis-aligned 1 mm grid, while the fMRI affine also reflects its larger voxel sizes and an oblique acquisition orientation.

Important distinction:

- **voxel coordinates**: indices in an image array;
- **world coordinates**: physical coordinates in millimetres;
- **registration/coregistration**: the later step used to align images accurately across spaces.

The affine is currently **in progress** as a learning topic. The next practical step is to transform one real fMRI voxel into world coordinates and map that physical location onto the T1w grid.

## Current project checkpoint

Completed:

- experiment and BIDS structure introduced;
- MRI, fMRI, BOLD and neurovascular coupling introduced;
- voxel, volume and TR understood;
- real NIfTI loaded with NiBabel;
- real fMRI shape and metadata inspected;
- real brain volume visualized;
- structural T1w image loaded and compared with fMRI;
- real affine matrices inspected.

In progress:

- spatial coordinates and affine interpretation.

Next:

```text
voxel index → affine → world coordinates → cross-image spatial correspondence
```

Only after the spatial/data foundations are secure will the project move toward:

```text
events → HRF → design matrix → GLM
```
