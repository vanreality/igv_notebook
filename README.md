# Genomic Track Viewer with IGV in Jupyter Notebook

This repository provides a Python script to visualize **BAM, BED, and VCF** files using [IGV Notebook](https://github.com/igvteam/igv-notebook) in a **Jupyter Notebook** environment.

## Features
- 📊 **Visualize BAM, BED, and VCF files** interactively in Jupyter.
- 🎨 **Color-coded labels** for easy track differentiation.
- ⚡ **Supports human genome (hg38)** (modifiable for other genomes).

---

## Installation

Ensure you have the required dependencies installed:

```bash
pip install pandas igv-notebook matplotlib
```

If using Jupyter Notebook, install `jupyterlab`:

```bash
pip install jupyterlab
```

---

## Usage

### 1️⃣ Import the Required Modules

### 2️⃣ Define the Function for IGV Visualization
The function `view_genomic_tracks(df_samplesheet)` takes a **Pandas DataFrame** as input and loads the genomic tracks into an **IGV Browser** inside a Jupyter Notebook.

### 3️⃣ Prepare Your Data
You need to create a Pandas DataFrame containing the following columns:
- `name`: Track name
- `label`: Sample label (used for color coding)
- `type`: File type (`bam`, `vcf`, `bed`)
- `path`: Absolute file path

### 4️⃣ Run the IGV Visualization
Call the function to visualize the genomic tracks inside a Jupyter notebook:

---

## Troubleshooting
- If files are not loading, ensure paths are correct.
- If IGV is not displaying tracks, restart Jupyter Kernel and retry.
- Ensure `igv-notebook` is installed.

---

## License
This project is licensed under the MIT License.

---

## Acknowledgments
- [IGV Notebook](https://github.com/igvteam/igv-notebook)
- [Matplotlib Colormaps](https://matplotlib.org/stable/users/prev_whats_new/whats_new_3.4.0.html#colormap-handling-improvements)