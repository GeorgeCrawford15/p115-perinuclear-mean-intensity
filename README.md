# periMI: Perinuclear Mean Intensity Analysis of p115

**Note:** This repository contains the periMI analysis pipeline. It is intended for postdoctoral use and may be cited in future publications.  

---

## Table of Contents

1. [Overview](#overview)  
2. [Inputs](#inputs)  
3. [Outputs](#outputs)  
4. [Installation](#installation)  
5. [Usage](#usage)  
6. [Configuration](#configuration)  
7. [License](#license)  
8. [Citation](#citation)  

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
- `cfg_1`, `cfg_2`, etc.: pipeline configuration objects

3. Run the notebook cells sequentially.
*Currently, the pipeline is designed for interactive use via Jupyter Notebook only.*

---

## Configuration

- All adjustable parameters are contained in the `PipelineConfig` dataclass, which controls image selection, regions of interest, and measurement settings.
- Users can create multiple configuration instances (`cfg_1`, `cfg_2`, etc.) to analyze different conditions.

---

## License

This repository is licensed under the MIT License. See [LICENSE(LICENSE)] for details.
- Users may freely use, modify, and redistribute the code.
- The copyright notice must be preserved in redistributed copies.

---

## Citation

- Once the associated manuscript is published, please cite the paper when using this code.
- Placeholder for citation:

```
Author(s), “Title,” Journal, Year. DOI
```
