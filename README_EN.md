# CL-RIQA & DTW Alignment Protocol

**Supplementary Code for Cathodoluminescence (CL) Image Analysis and Spatiotemporal Alignment with LA-ICP-TOF-MS Data**

---

## 📁 File Structure

```
Supplementary_Code/
├── 01-cl_riqa_&_dtw_alignment_protocol.py    # Main Python script
├── 02-1-Input_dataset_1_CL_image.jpg         # Input: CL micrograph
├── 02-2-Input_dataset_2_CL_Data_696um_5um_Mn-Fe.csv  # Input: LA-ICP-MS data
└── README.md                                  # This file
```

---

## 🔬 Overview

This protocol implements **CL Reconstructed-Image Quantitative Analysis (CL-RIQA)** combined with **Dynamic Time Warping (DTW)** to:
1. Extract quantitative CL intensity profiles from optical micrographs
2. Align CL profiles with LA-ICP-TOF-MS chemical data (Mn/Fe ratios)
3. Correct spatial offsets caused by aerosol-transport lag and washout effects
4. Validate geochemical correlations using statistical methods

**Reference**: See main manuscript Section X.X and Supplementary Text S2 for detailed methodology.

---

## 📋 Prerequisites

### Python Environment
- **Python version**: ≥ 3.7
- **Recommended**: Anaconda or Miniconda

### Required Libraries
Install dependencies using pip:

```bash
pip install numpy pandas matplotlib seaborn opencv-python scipy scikit-learn fastdtw
```

Or install from `requirements.txt` (if provided):
```bash
pip install -r requirements.txt
```

**Library versions used in this study**:
- numpy >= 1.21.0
- pandas >= 1.3.0
- matplotlib >= 3.4.0
- opencv-python >= 4.5.0
- scipy >= 1.7.0
- scikit-learn >= 0.24.0
- fastdtw >= 0.3.4

---

## 🚀 Execution Instructions

### **Step 1: Prepare Input Files**

Ensure the following files are in the **same directory** as the Python script:

1. **CL Image** (`.jpg` or `.png`)
   - Filename: `02-1-Input_dataset_1_CL_image.jpg`
   - Format: Color CL micrograph (8-bit RGB)
   - Requirement: Red channel should capture Mn²⁺-activated luminescence

2. **LA-ICP-MS Data** (`.csv`)
   - Filename: `02-2-Input_dataset_2_CL_Data_696um_5um_Mn-Fe.csv`
   - Required column: **`Mn/Fe`** (case-sensitive)
   - Format: Numeric values representing Mn/Fe atomic ratios along the ablation transect

**Example CSV structure**:
```csv
Bin_Range_um,Distance_Center_um,Mn/Fe
0.0-5.0,2.5,0.144272
5.0-10.0,7.5,0.138022
...
```

---

### **Step 2: Execute the Script**

#### **Option A: Standard Python Execution (Recommended for Reviewers)**

If you have modified the script to use standard file I/O:

```bash
python 01-cl_riqa_&_dtw_alignment_protocol.py
```

The script will automatically load:
- `02-1-Input_dataset_1_CL_image.jpg`
- `02-2-Input_dataset_2_CL_Data_696um_5um_Mn-Fe.csv`

#### **Option B: Google Colab Execution (Original Version)**

1. Upload `01-cl_riqa_&_dtw_alignment_protocol.py` to Google Colab
2. Run all cells
3. When prompted, upload the two input files:
   - First prompt: Upload `02-1-Input_dataset_1_CL_image.jpg`
   - Second prompt: Upload `02-2-Input_dataset_2_CL_Data_696um_5um_Mn-Fe.csv`

---

### **Step 3: Check Outputs**

The script will generate the following files in the working directory:

| Output File | Description |
|-------------|-------------|
| `Figure_Comparison.svg` | High-resolution vector figure (4-panel comparison) |
| `Figure_Comparison.pdf` | PDF version of the figure (for publication) |
| `CL_MS_Comparison_Data.csv` | Processed data (raw and aligned profiles) |

**Figure panels**:
- **(a)** Spatial profiles before alignment (CL vs. Mn/Fe)
- **(b)** Spatial profiles after DTW alignment
- **(c)** Correlation plot (raw data) with R² statistics
- **(d)** Correlation plot (aligned data) with R² statistics

---

## 🔧 Algorithm Parameters

Key parameters are defined in **Table A6** (Supplementary Materials):

| Parameter | Value | Description |
|-----------|-------|-------------|
| **I_bg** (Background threshold) | 20 (8-bit) | Pixels with I ≤ 20 are excluded as epoxy resin background |
| **I_std** (Normalization standard) | 200 (8-bit) | Reference intensity for CL Index calculation |
| **Median filter kernel** | 3×3 | Denoising filter size |
| **Supersampling factor** | 5× | Resolution enhancement for DTW |
| **DTW radius constraint** | 10% of resampled length | Prevents pathological warping |
| **Random Forest settings** | n_estimators=50, CV=5, random_state=42 | Validation parameters |

---

## 📊 Expected Results

Using the provided example dataset (`02-1` and `02-2`):

- **Pre-alignment R²**: Typically < 0.3 (poor correlation)
- **Post-alignment R²**: Typically > 0.7 (strong correlation)
- **Processing time**: ~10-30 seconds (depending on image size and data length)

**Interpretation**: Successful alignment demonstrates that CL intensity variations are controlled by Mn/Fe compositional changes, validating the use of CL imaging for diagenetic analysis.

---

## ⚠️ Troubleshooting

### Common Issues:

1. **Error: "Column 'Mn/Fe' not found"**
   - Check that your CSV file contains a column named exactly `Mn/Fe`
   - Column names are case-sensitive

2. **Error: "Image decoding failed"**
   - Ensure the image file is a valid JPG or PNG
   - Check that the file is not corrupted

3. **Poor alignment results (low R² after alignment)**
   - Verify that the CL image and MS data are from the same transect
   - Check for data quality issues (e.g., excessive noise, missing values)

4. **Import errors**
   - Reinstall dependencies: `pip install --upgrade [package_name]`
   - For Google Colab users: Add `!pip install fastdtw` at the beginning

---

## 📖 Citation

If you use this code in your research, please cite:

> [Author et al. (Year). Article Title. *Journal Name*, Volume(Issue), Pages. DOI: xxx]

---

## 📧 Contact

For questions or issues related to this code, please contact:

- **Corresponding Author**: [Fei Li] ([lifei@swpu.edu.cn])
- **Code Developer**: [Fei Li; Code development assisted by large language models Claude and Gemini
for optimization and documentation] ([lifei@swpu.edu.cn])

---

## 📄 License

This code is provided as supplementary material for academic research purposes. 
Redistribution and use in source and binary forms are permitted for non-commercial 
applications with proper citation of the original publication.

---

## 🔄 Version History

- **v1.0** (2026-02-02): Initial release with manuscript submission

---

**Last Updated**: February 2, 2026
