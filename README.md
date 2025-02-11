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
```python
import pandas as pd
import igv_notebook
import os
import matplotlib.pyplot as plt
import matplotlib.colors as mcolors
```

### 2️⃣ Define the Function for IGV Visualization
The function `view_genomic_tracks(df_samplesheet)` takes a **Pandas DataFrame** as input and loads the genomic tracks into an **IGV Browser** inside a Jupyter Notebook.

```python
def view_genomic_tracks(df_samplesheet):
    """
    Visualize BAM, BED, and VCF files using igv_notebook in a Jupyter notebook.

    Parameters:
    df_samplesheet (pd.DataFrame): Input dataframe with columns ['name', 'label', 'type', 'path'].

    Returns:
    None
    """
    # Ensure required columns exist
    required_columns = ['name', 'label', 'type', 'path']
    if not all(col in df_samplesheet.columns for col in required_columns):
        raise ValueError(f"DataFrame must contain columns: {required_columns}")

    # Check for file existence
    for file_path in df_samplesheet['path']:
        if not os.path.exists(file_path):
            raise FileNotFoundError(f"File not found: {file_path}")

    # Generate a unique list of labels
    unique_labels = df_samplesheet['label'].unique()

    # Create a colormap for labels
    colormap = plt.colormaps["tab10"](range(len(unique_labels)))
    label_colors = {label: mcolors.to_hex(colormap[i]) for i, label in enumerate(unique_labels)}

    # Initialize IGV
    igv_notebook.init()
    browser = igv_notebook.Browser(
        {
            "genome": "hg38",  # Default genome; adjust as needed
            "tracks": [],
        }
    )

    # Parse the DataFrame and create tracks
    for _, row in df_samplesheet.iterrows():
        track = {
            "name": row['name'],
            "type": row['type'],  # bam, bed, or vcf
            "url": os.path.abspath(row['path']),
            "color": label_colors[row['label']],
        }
        browser.load_track(track)

    # Display the browser
    display(browser)
```

### 3️⃣ Prepare Your Data
You need to create a Pandas DataFrame containing the following columns:
- `name`: Track name
- `label`: Sample label (used for color coding)
- `type`: File type (`bam`, `vcf`, `bed`)
- `path`: Absolute file path

```python
df1 = pd.DataFrame(
    data={
        "name":["T1188P_raw"],
        "label":["T1188P"],
        "type":["bam"],
        "path":["/path/to/T1188P.raw.bam"]
    }
)

df2 = pd.DataFrame(
    data={
        "name":["T1188P_preprocessed"],
        "label":["T1188P"],
        "type":["bam"],
        "path":["/path/to/T1188P.preprocessed.bam"]
    }
)

df3 = pd.DataFrame(
    data={
        "name":["T1188P_vcf"],
        "label":["T1188P"],
        "type":["vcf"],
        "path":["/path/to/T1188P.snpsift.vcf"]
    }
)

merged_df = pd.concat([df1, df2, df3], axis=0)
```

### 4️⃣ Run the IGV Visualization
Call the function to visualize the genomic tracks inside a Jupyter notebook:

```python
view_genomic_tracks(merged_df)
```

---

## Example Output
When executed in Jupyter Notebook, this script loads the specified **BAM, BED, or VCF** files into **IGV Notebook**, displaying aligned sequencing reads and variants.

![Example IGV](https://raw.githubusercontent.com/example/igv_notebook_demo.png)

---

## Troubleshooting
- If files are not loading, ensure paths are correct using:
  ```python
  import os
  print(os.path.abspath("/path/to/T1188P.raw.bam"))
  ```
- If IGV is not displaying tracks, restart Jupyter Kernel and retry.
- Ensure `igv-notebook` is installed.

---

## License
This project is licensed under the MIT License.

---

## Acknowledgments
- [IGV Notebook](https://github.com/igvteam/igv-notebook)
- [Matplotlib Colormaps](https://matplotlib.org/stable/users/prev_whats_new/whats_new_3.4.0.html#colormap-handling-improvements)