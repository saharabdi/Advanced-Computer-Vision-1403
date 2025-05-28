# Satellite Image Analysis: Spectral Indices (NDWI, MNDWI, NDVI) with Google Earth Engine

This repository provides resources for understanding and calculating common spectral indices used in remote sensing for environmental analysis. It includes theoretical explanations of the Normalized Difference Water Index (NDWI), Modified Normalized Difference Water Index (MNDWI), and Normalized Difference Vegetation Index (NDVI), along with a Python script demonstrating their computation and visualization using Google Earth Engine (GEE) and Landsat 8 satellite imagery.

## Table of Contents

1.  [Spectral Indices Explained](#-spectral-indices-explained)
    * [NDWI (Normalized Difference Water Index)](#-1-ndwi-normalized-difference-water-index)
    * [MNDWI (Modified Normalized Difference Water Index)](#-2-mndwi-modified-normalized-difference-water-index)
    * [NDVI (Normalized Difference Vegetation Index)](#-3-ndvi-normalized-difference-vegetation-index)
2.  [Illustrative Example: Index Calculation & Analysis](#-illustrative-example-index-calculation--analysis)
3.  [Google Earth Engine Implementation](#-google-earth-engine-implementation)
    * [Overview](#overview)
    * [Requirements](#requirements)
    * [Setup](#setup)
    * [Running the Script](#running-the-script)
    * [Key Script Features](#key-script-features)
4.  [Expected Output](#-expected-output)

---

## ? Spectral Indices Explained

Spectral indices are combinations of spectral reflectance values from two or more wavelength bands that highlight specific phenomena on the Earth's surface, such as vegetation, water, or built-up areas.

### ?? 1. NDWI (Normalized Difference Water Index)

The NDWI is used to delineate open water features.

**General Formula:**

$$
\text{NDWI} = \frac{\text{Green} - \text{NIR}}{\text{Green} + \text{NIR}}
$$

**Band Rationale:**

* **Green Band:** Reflects well from water surfaces and vegetation.
* **Near-Infrared (NIR) Band:** Strongly absorbed by water, making water bodies appear dark. Vegetation and soil, on the other hand, reflect NIR significantly, appearing bright. This contrast is key to isolating water.

**Specific Bands for Common Sensors:**

* **Sentinel-2:** $\text{NDWI} = \frac{B3 - B8}{B3 + B8}$
    * B3: Green
    * B8: NIR
* **Landsat 8/9 (as used in the GEE script):** $\text{NDWI} = \frac{B3 - B5}{B3 + B5}$
    * B3: Green
    * B5: NIR

---

### ?? 2. MNDWI (Modified Normalized Difference Water Index)

The MNDWI enhances the delineation of open water features while suppressing built-up land noise.

**General Formula:**

$$
\text{MNDWI} = \frac{\text{Green} - \text{SWIR}}{\text{Green} + \text{SWIR}}
$$

**Key Difference from NDWI & Advantages:**

* MNDWI substitutes the NIR band with a **Short-Wave Infrared (SWIR)** band.
* **Why SWIR?** Water absorbs SWIR radiation even more strongly than NIR. Soil, vegetation, and especially built-up land features tend to have higher reflectance in the SWIR region compared to water.
* **Advantages of MNDWI:**
    * **Enhanced Water Feature Extraction:** Often provides better discrimination of water bodies.
    * **Reduced Noise from Built-up Areas:** Built-up land can have NIR reflectance similar to water, leading to misclassification by NDWI. MNDWI effectively suppresses signals from these areas.
    * **Improved Performance with Turbid Water:** Can offer better delineation of turbid water compared to NDWI.

**When to Use MNDWI:**

* Particularly useful in **urban areas** or regions with significant man-made structures.

**Specific Bands for Common Sensors:**

* **Sentinel-2:** $\text{MNDWI} = \frac{B3 - B11}{B3 + B11}$
    * B3: Green
    * B11: SWIR (typically SWIR1 range)
* **Landsat 8/9 (as used in the GEE script):** $\text{MNDWI} = \frac{B3 - B6}{B3 + B6}$
    * B3: Green
    * B6: SWIR1

---

### ?? 3. NDVI (Normalized Difference Vegetation Index)

The NDVI is a widely used index to quantify vegetation greenness and health.

**General Formula:**

$$
\text{NDVI} = \frac{\text{NIR} - \text{Red}}{\text{NIR} + \text{Red}}
$$

**Band Rationale:**

* **Red Band:** Chlorophyll in healthy vegetation absorbs red light for photosynthesis.
* **Near-Infrared (NIR) Band:** Healthy vegetation strongly reflects NIR light.
* Higher NDVI values indicate denser and healthier vegetation. Values near zero or negative typically indicate non-vegetated surfaces like bare soil, water, snow, or clouds.

**Specific Bands for Common Sensors:**

* **Sentinel-2:** $\text{NDVI} = \frac{B8 - B4}{B8 + B4}$
    * B4: Red
    * B8: NIR
* **Landsat 8/9 (as used in the GEE script):** $\text{NDVI} = \frac{B5 - B4}{B5 + B4}$
    * B4: Red
    * B5: NIR

---

## ?? Illustrative Example: Index Calculation & Analysis

This example uses hypothetical pixel values, representative of bands from a sensor like Sentinel-2, to demonstrate index interpretation.

**Given Pixel Reflectance Values:**

* Green (e.g., Sentinel-2 B3): 1
* NIR (e.g., Sentinel-2 B8): 9
* SWIR (e.g., Sentinel-2 B11): 12
* Red (e.g., Sentinel-2 B4): 9

**Calculations:**

* **NDWI:**
    $$
    \text{NDWI} = \frac{1 - 9}{1 + 9} = \frac{-8}{10} = \mathbf{-0.8}
    $$

* **MNDWI:**
    $$
    \text{MNDWI} = \frac{1 - 12}{1 + 12} = \frac{-11}{13} \approx \mathbf{-0.846}
    $$

* **NDVI:**
    $$
    \text{NDVI} = \frac{9 - 9}{9 + 9} = \frac{0}{18} = \mathbf{0}
    $$

**Analysis:**

* **NDWI = -0.8:** Indicates a strong absence of water. NDWI values for water are typically positive.
* **MNDWI = -0.846:** Further confirms the absence of water, potentially indicating urban/built-up land or dry soil, as MNDWI is sensitive to these features.
* **NDVI = 0:** Suggests a non-vegetated surface. Healthy vegetation yields high positive NDVI values (0.2 to 0.9). An NDVI of 0 (where Red reflectance equals NIR reflectance) typically points to bare soil, rock, sand, or artificial surfaces.

**Conclusion from Example:** The pixel likely represents a **dry, urban, or bare soil area** with no significant water or vegetation presence.

---

## ??? Google Earth Engine Implementation

The provided Python script (`your_script_name.py`) demonstrates how to calculate NDWI, MNDWI, and NDVI using Landsat 8 imagery within Google Earth Engine and visualize the results.

### Overview

The script performs the following actions:
1.  Initializes the Google Earth Engine API.
2.  Defines an Area of Interest (AOI) around Bandar Anzali, Iran.
3.  Fetches a cloud-free Landsat 8 Top-of-Atmosphere (TOA) image for the AOI and a specified date range.
4.  Calculates NDVI, NDWI, and MNDWI using the appropriate Landsat 8 bands.
5.  Displays an interactive map using `geemap` with layers for True Color, NDVI, NDWI, and MNDWI.
6.  Generates and saves a static image (`bandar_anzali_indices.png`) showing these layers.

*Note: The script uses a future date range (`'2025-01-01', '2025-03-30'`) for image acquisition. You may need to adjust these dates to find available imagery for your analysis period.*

### Requirements

* Python 3.x
* A Google Earth Engine account.
* Authenticated GEE Python API.
* The following Python libraries:
    * `earthengine-api`
    * `geemap`
    * `matplotlib`

### Setup

1.  **Install Libraries:**
    ```bash
    pip install earthengine-api geemap matplotlib --quiet
    ```

2.  **Authenticate and Initialize Earth Engine:**
    When you run the script for the first time, or if your credentials have expired, `geemap` or `ee` will guide you through an authentication process.
    The script includes:
    ```python
    try:
        ee.Initialize(project='your-gee-project-id') # Replace with your GEE project ID
    except Exception:
        ee.Authenticate()
        ee.Initialize(project='your-gee-project-id') # Replace with your GEE project ID
    ```
    **Important:** Replace `'gen-lang-client-0219770664'` in the script with your own Google Earth Engine Project ID.

### Running the Script

Execute the Python script from your terminal:
```bash
python your_script_name.py