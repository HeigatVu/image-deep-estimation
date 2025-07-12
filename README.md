# Stereo Matching for Depth Estimation

This project explores **Stereo Matching**, a fundamental computer vision technique used to recover 3D depth information from a pair of 2D images. The primary goal is to implement and compare different algorithms for generating a **disparity map**, which encodes depth information for each pixel. [cite_start]This project is based on the "Image Depth Estimation" module from the AI VIET NAM AI COURSE 2024. [cite: 1]

[cite_start]The generated disparity maps can be applied in various fields, including **Autonomous Driving** and **Augmented Reality**. [cite: 7]

---

##  notebooks and Usage

This project's implementations are organized into two Jupyter Notebooks. To see the results, open and run the cells within each notebook. The code will process the images and save the resulting disparity maps as `.png` files in the main directory.

### `pixel-wise-matching.ipynb`

This notebook covers **Problem 1** of the project.
* It contains the Python code to perform pixel-wise stereo matching.
* [cite_start]Implements both **L1 (Absolute Difference)** and **L2 (Squared Difference)** cost functions. [cite: 144, 145]
* [cite_start]Running this notebook will generate the disparity maps for the **Tsukuba** dataset. [cite: 16]

### `window-based-matching.ipynb`

This notebook addresses **Problems 2, 3, and 4**.
* It implements the more robust window-based matching algorithm.
* You will find the code for:
    * [cite_start]Window-based matching with **L1 and L2** costs (Problem 2). [cite: 336, 337]
    * [cite_start]The setup for analyzing matching failures (Problem 3). [cite: 399]
    * [cite_start]The improved matching algorithm using **Cosine Similarity** (Problem 4). [cite: 400]
* [cite_start]This notebook uses the **Aloe** dataset for its examples. [cite: 17]

---

## ⚙️ Algorithms Explained

#### Pixel-Wise Matching
This is the most fundamental approach. [cite_start]The disparity for each pixel is calculated independently by finding the best match along the corresponding horizontal line in the other image within a specified `disparity_range`. [cite: 23]

#### Window-Based Matching
To improve upon the noise sensitivity of the pixel-wise method, this approach compares a window of pixels (`k x k`) instead of a single pixel. [cite_start]The cost is aggregated over the entire window for a more robust match, resulting in a smoother disparity map. [cite: 191, 533]

#### Cosine Similarity Matching
To address issues where lighting and exposure differences cause problems for L1/L2 norms, this metric is used. [cite_start]It measures the cosine of the angle between two window vectors, making it robust to illumination changes. [cite: 400]

---

## 📚 Datasets

This project utilizes two stereo image datasets:
* [cite_start]**Tsukuba**: Used for Problem 1. [cite: 16]
* [cite_start]**Aloe**: Used for Problems 2, 3, and 4. [cite: 17]
