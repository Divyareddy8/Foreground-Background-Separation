# Foreground–Background Separation using Optimization

This project implements and analyzes optimization-based methods for reliably separating moving foreground elements from static backgrounds in video sequences.

---

## Project Overview

The objective is to automate the extraction of clean foreground objects from complex video data, which is essential for applications such as surveillance, traffic monitoring, and robotics.  
The problem is addressed at two levels of complexity:

### Easy Problem
Uses temporal pixel statistics (Mean and Median filters) for static or mildly dynamic scenes.

### Hard Problem
Employs **Robust Principal Component Analysis (RPCA)** to decompose video data into low-rank (background) and sparse (foreground) components.

---

## Dataset

We use the **CDnet 2014 (Change Detection)** dataset, which contains diverse real-world scenarios including highways, camera jitter, dynamic backgrounds, and shadows.

**Source:** https://www.kaggle.com/datasets/maamri95/cdnet2014

**Categories tested:**
- Baseline (Highway)
- CameraJitter (Sidewalk)
- DynamicBackground (Canoe)
- Shadow (BusStation)

---

## Algorithms Implemented

### 1. Mean and Median Filters (Statistical Methods)

**Mean Filter**
- Models each pixel as a Gaussian random variable.
- Uses exponentially weighted moving averages for adaptive background updates.

**Median Filter**
- Robust to short-term disturbances.
- Estimates the median intensity at each pixel location over time.

---

### 2. Robust Principal Component Analysis (Optimization Method)

**Formulation**  
The video sequence is reshaped into a matrix and decomposed as:

```

D = L + S

```

- `L`: Low-rank matrix representing the background  
- `S`: Sparse matrix representing moving foreground objects  

**Optimization**  
The decomposition is solved using the **Inexact Augmented Lagrange Multiplier (IALM)** method by minimizing the nuclear norm of `L` and the `l1` norm of `S`.

---

## Performance Results (Highway – Baseline Category)

| Algorithm      | Precision | Recall | F1-Score | IoU    | Execution Time |
|---------------|-----------|--------|----------|--------|----------------|
| Mean Filter   | 0.6712    | 0.9031 | 0.7701   | 0.6261 | 83.93 s        |
| Median Filter | 0.6753    | 0.6130 | 0.6426   | 0.4734 | 39.60 s        |
| RPCA          | 0.8137    | 0.8289 | 0.8212   | 0.6967 | 285.16 s       |

---

## Key Takeaways

- **RPCA** achieves the highest accuracy and robustness to noise and dynamic backgrounds, but is computationally expensive (approximately 7 times slower than the Median filter).
- **Mean and Median filters** are suitable for real-time or resource-constrained systems due to their low computational cost, but they are sensitive to noise, shadows, and ghosting artifacts.
- **Trade-off between realism and efficiency:** Handling complex, real-world scenes requires either increased computational resources or more expressive background models (higher effective rank).

---

