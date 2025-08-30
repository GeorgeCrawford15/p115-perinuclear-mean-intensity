# periMI: Perinuclear Mean Intensity Analysis of p115

## Table of Contents

1. [Overview](#overview)  
2. [Inputs](#inputs)  
3. [Outputs](#outputs)  
4. [Installation](#installation)  
5. [Usage](#usage)  
6. [Configuration](#configuration)  
7. [License](#license)

---

## Overview

`periMI` is a Jupyter Notebook-based pipeline for analyzing the spatial distribution of p115 protein intensity relative to the nucleus in microscopy images. The pipeline calculates **perinuclear mean intensity**, **maximum intensity**, and **area measurements**, providing quantitative insights into protein localization.  

> *Note:* Specific experimental context is restricted until associated publications are released.  

---

## Inputs

- A single folder specified in the pipeline variable `img_sets`.  
- Within this folder: multiple subfolders (**image sets**) containing TIFF microscopy images.  
- Image naming convention within each set:  
  - `z<number>`: z-plane index (0–7)  
  - `ch00`: nucleus channel  
  - `ch01`: transfection marker channel  
  - `ch02`: p115 channel  

The pipeline iterates over each image set, selecting images based on parameters specified in the user’s `PipelineConfig` dataclass instance (e.g., `cfg_1`, `cfg_2`).  

---

## Outputs

- **Per-image-set Pandas DataFrames:** analysis results for each image set.  
- **Summary DataFrame:** aggregates key summary rows from each image set.  
- **Excel file:** primary output, saved to the folder specified by `output_folder_path`. Each image set DataFrame is saved in its own sheet.  

---

## Installation

1. Clone this repository:  
```bash
git clone https://github.com/<your-username>/p115-perinuclear-mean-intensity.git
cd p115-perinuclear-mean-intensity
```

2. Create a Conda environment from `environment.yml`:
```bash
conda env create -f environment.yml
conda activate periMI
```

3. Launch Jupyter Lab:
```bash
jupyter lab
```

---

## Usage

1. Open `periMI.ipynb` in Jupyter Lab.

2. Set the following variables:
- `img_sets`: path to your top-level folder containing image sets
- `output_folder_path`: path to save Excel outputs
- `cfg_1`, `cfg_2`, etc.: pipeline configuration objects. Include the specific parameter values you want instead of the following values in the default settings:
  - `show_viewer: bool = True` &rarr; Generates a Napari window for each image set. Toggling this to False is recommended when analyzing many image sets.
  - `nucleus_plane: int = 3` &rarr; Keep between 1 and 6. Specifies the z-plane index (`z<number>`) of the desired nucleus image. p115 analysis is done on 3 p115 images: 1 plane below the nucleus image, on the same plane as the nucleus image, and 1 plane above the nucleus image.
  - `erosion_intensity: int = 1` &rarr; Set to 2 or 3 for more intense image erosion. 
  - `sigma_est_toggle: bool = True` &rarr; Estimated noise standard deviation. This value multiplies the filter strength to provide a tailored denoising strength. Keep this as False if you don't want it to influence denoising and want more manual control over the denoising strength.
  - `denoising_strength: float = 0.8` &rarr; This number will be multiplied by sigma_est if sigma_est is set to True, and thus the denoising strength will be 80% of the standard deviation (or 70% if you change this number to 0.7, and so on). Otherwise, this number will solely define the denoising strength.
  - `denoising_patch_size: int = 11` &rarr; Larger value = captures more context; better denoising but loses fine detail (which might not matter in some images, but may cause bad nucleus masking in other images with darker nucleus pixels).
  - `denoising_patch_distance: int = 12` &rarr; Larger value = searches wider area, so likely better denoising but slower processing.
  - `denoising_fast_mode: bool = True` &rarr; Make False for slower but potentially more accurate denoising.
  - `nucleus_mask_threshold: float = 0.05` &rarr; Keep between 0 and 1. Specifies the minimum intensity a pixel must have to be considered a part of a nucleus. 
  - `nucleus_area_threshold: int = 500` &rarr; Removes regions in the nucleus mask that are smaller than 500 pixels.
  - `desired_donut_width_um: float = 4.5` &rarr; Specifies each cell's inner donut width in microns. Tweaking this value will likely change the analysis results drastically.
  - `desired_outer_donut_width_um: float = 1.5` &rarr; Specifies each cell's outer donut width in microns. Tweaking this value will likely change the analysis results drastically.
  - `overlap_filter: str = 'lenient'` &rarr; Setting to 'lenient' deletes all cell regions whose outer donuts overlap with other regions' inner donuts, and vice versa (recommended). Setting to 'strict' deletes all cell regions that overlap.
  - `lower_transfection_intensity_threshold: float = 0.05` &rarr; Keep between 0 and 1. Cell regions whose inner donuts have mean transfection intensities lower than this number are removed.
  - `upper_transfection_intensity_threshold: float = 0.2` &rarr; Keep between 0 and 1. Cell regions whose inner donuts have mean transfection intensities higher than this number are removed.
  - `p115_area_intensity_threshold: float = 0.05` &rarr; Keep between 0 and 1. Pixels in each analyzed p115 image whose intensities are lower than this number are not considered when calculating the total area of p115 (in pixels) for each mask in each cell region.

3. Run the notebook cells sequentially.
*Currently, the pipeline is designed for interactive use via Jupyter Notebook only.*

---

## Configuration

- All adjustable parameters are contained in the `PipelineConfig` dataclass, which controls image selection, filtering, and measurement settings.
- Users can create multiple configuration instances (`cfg_1`, `cfg_2`, etc.) to analyze different conditions.

---

## License

This repository is licensed under the MIT License. See [LICENSE](LICENSE) for details.
- Users may freely use, modify, and redistribute the code.
- The copyright notice must be preserved in redistributed copies.
