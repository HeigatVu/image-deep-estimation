# Image Depth Estimation using Stereo Matching Algorithms

This project explores fundamental computer vision techniques for estimating depth from a pair of 2D stereo images. By implementing and comparing several stereo matching algorithms, we generate a **disparity map** that represents the 3D structure of a scene. This is a core task in applications like autonomous driving, robotics, and augmented reality.

---

## 📖 Methodologies Implemented

The goal of stereo matching is to find the corresponding pixel in the right image for each pixel in the left image. The horizontal displacement between these corresponding pixels is the "disparity." A higher disparity value means the object is closer to the camera.

This project implements three classical methods to calculate the disparity map.

### 1. Pixel-wise Matching

This is the most fundamental approach. For each pixel in the left image, it searches for the best matching pixel along the same horizontal line (epipolar line) in the right image.

* **Process**:
    1.  For each pixel `L(y, x)` in the left image, a "cost" is calculated by comparing it to a range of pixels `R(y, x - d)` in the right image, where `d` is the disparity being tested (`d` is from 0 to `disparity_range`).
    2.  The disparity `d_optimal` that results in the minimum cost is chosen for that pixel.
    3.  This process is repeated for all pixels to build the disparity map.
* **Cost Functions Used**:
    * **L1 Distance (Sum of Absolute Differences)**: `cost = |L(y, x) - R(y, x - d)|`
    * **L2 Distance (Sum of Squared Differences)**: `cost = (L(y, x) - R(y, x - d))^2`
* **Limitation**: This method is very sensitive to noise and performs poorly in textureless regions, as individual pixel values are not unique enough.

### 2. Window-based Matching

This method improves upon pixel-wise matching by comparing a *window* of pixels instead of a single pixel. This makes the matching process more robust and stable.

* **Process**:
    1.  For each pixel `L(y, x)`, a `k x k` window (kernel) is centered around it.
    2.  This window from the left image is compared to corresponding windows in the right image, shifted by disparity `d`.
    3.  The cost is the sum of all the pixel-wise differences within the entire window.
    4.  The disparity `d_optimal` that gives the minimum total cost for the window is chosen.
* **Benefit**: By aggregating information from a local neighborhood, this method produces smoother and more reliable disparity maps compared to the pixel-wise approach.

### 3. Window-based Matching with Cosine Similarity

This method addresses a key weakness of L1/L2 distances: sensitivity to lighting and exposure differences between the left and right images. Instead of measuring the difference in pixel intensity, it measures the similarity in the *pattern* or *structure* of the pixel values.

* **Process**:
    1.  The `k x k` windows from the left and right images are "flattened" into vectors.
    2.  The **Cosine Similarity** between the two vectors is calculated. This measures the angle between them, ignoring their magnitude (i.e., overall brightness).
        $cosine\_similarity(\vec{x}, \vec{y}) = \frac{\vec{x} \cdot \vec{y}}{||\vec{x}|| \cdot ||\vec{y}||}$
    3.  The disparity `d_optimal` that results in the *maximum* similarity score is chosen.
* **Benefit**: This approach is highly effective at finding correct correspondences even when the two stereo images have different brightness levels, leading to more accurate results in challenging lighting conditions.

---

## 📈 Results

The project generates both grayscale and color-coded disparity maps for easy visualization.

* **Pixel-wise matching** produces noisy results, especially on the Tsukuba dataset.
* **Window-based matching** significantly reduces noise and provides a much smoother map for the Aloe dataset.
* **Cosine Similarity** proves to be the most robust method, successfully handling the lighting variations in the more challenging Aloe image pairs and producing the cleanest disparity map.

| Method | L1 Result (Aloe) | L2 Result (Aloe) | Cosine Similarity Result (Aloe) |
| :--- | :---: | :---: | :---: |
| **Disparity Map** | ![L1](https://github.com/HeigatVu/image-deep-estimation/blob/main/window_based_l1_color.png) | ![L2](https://github.com/HeigatVu/image-deep-estimation/blob/main/window_based_l2_color.png) | ![Cosine](https://github.com/HeigatVu/image-deep-estimation/blob/main/window_based_cs_color.png) |
| **Notes** | Noisy, less defined edges. | Similar to L1 but slightly different noise pattern. | Smoothest result, clear object definition, robust to lighting. |

---

## 🛠️ Technologies Used

* **Python**
* **OpenCV-Python**: Used for all core image processing tasks, including reading images, color space conversions, and saving results.
* **NumPy**: Used for efficient array manipulation and mathematical calculations.

## 🚀 How to Run

1.  **Clone the repository**.
2.  **Install dependencies**:
    ```bash
    pip install opencv-python numpy
    ```
3.  **Run the main script** from your terminal, providing the required arguments:
    * Paths to the left and right stereo images.
    * `disparity_range`: The maximum disparity to search for.
    * `kernel_size`: The size of the window for window-based methods.
4.  The script will save the resulting disparity maps (both grayscale and color) in the project directory.
