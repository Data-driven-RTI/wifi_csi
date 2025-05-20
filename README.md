# Project Data Requirements

This document outlines the expected data types and directory structure for the input data used by the `CsiDataset` and `RssiDataset` classes in the `train_evaluate_combined.py` script.

## General Structure

The script expects preprocessed data to be stored as NumPy arrays (`.npy` files). For each data sample, there should be two corresponding files:

1.  **Feature File:** Contains the input signal data (CSI or RSSI).
2.  **Ground Truth Mask File:** Contains the corresponding segmentation mask.

These pairs of files should be discoverable by the respective dataset classes, typically by residing in the same directory and potentially following a consistent naming convention that allows the dataset class to pair them (e.g., `sample01_features.npy` and `sample01_gt.npy`).

## 1. CSI Data (`CsiDataset`)

*   **Purpose:** Loads Channel State Information (CSI) data.
*   **Directory:** Pointed to by the `--csi_data_dir` argument (e.g., `./processed_data_csi/`).
*   **Feature File (`.npy`):**
    *   **Data Type:** `numpy.float32`
    *   **Shape:** Expected to be `(C, H, W)`
        *   `C`: Number of input channels. For the current script configuration, this is typically `14 * 52 = 728` (representing, for example, 14 antennas/links each with 52 subcarrier amplitude values).
        *   `H`: Height of the feature map (e.g., `16` in the script's example `feature_shape`).
        *   `W`: Width of the feature map (e.g., `16` in the script's example `feature_shape`).
    *   **Content:** Preprocessed CSI amplitude (or phase, or combined) values, spatially arranged or mapped into a 2D grid format per channel.
*   **Ground Truth Mask File (`.npy`):**
    *   **Data Type:** `numpy.float32` (values typically normalized between 0 and 1).
    *   **Shape:** Expected to be `(Target_H, Target_W)` or `(1, Target_H, Target_W)`.
        *   `Target_H`: Height of the ground truth mask (e.g., `360` as per script defaults).
        *   `Target_W`: Width of the ground truth mask (e.g., `360` as per script defaults).
    *   **Content:** A 2D segmentation mask where pixel values represent the probability or binary presence of the target class.

## 2. RSSI Data (`RssiDataset`)

*   **Purpose:** Loads Received Signal Strength Indicator (RSSI) data.
*   **Directory:** Pointed to by the `--rssi_data_dir` argument (e.g., `./processed_data_rssi/`).
*   **Feature File (`.npy`):**
    *   **Data Type:** `numpy.float32`
    *   **Shape:** Expected to be `(C, H, W)`
        *   `C`: Number of input channels. For the current script configuration, this is typically `14` (representing, for example, RSSI values from 14 antennas/links).
        *   `H`: Height of the feature map (e.g., `16`).
        *   `W`: Width of the feature map (e.g., `16`).
    *   **Content:** Preprocessed RSSI values, spatially arranged or mapped into a 2D grid format per channel.
*   **Ground Truth Mask File (`.npy`):**
    *   **Data Type:** `numpy.float32` (values typically normalized between 0 and 1).
    *   **Shape:** Expected to be `(Target_H, Target_W)` or `(1, Target_H, Target_W)`.
        *   `Target_H`: Height of the ground truth mask (e.g., `360`).
        *   `Target_W`: Width of the ground truth mask (e.g., `360`).
    *   **Content:** A 2D segmentation mask.

## Notes:

*   The exact interpretation of `C` (channels), `H` (height), and `W` (width) for the feature files depends on your specific preprocessing pipeline that generates these `.npy` files. The dataset classes in the script are configured with example shapes (`(728, 16, 16)` for CSI features, `(14, 16, 16)` for RSSI features) which might need to be adjusted in the script or your data preprocessing if your data differs.
*   The ground truth masks should spatially correspond to the area you are trying to segment.
*   The dataset classes in `train_evaluate_combined.py` (specifically `CsiDataset` and `RssiDataset`) are responsible for loading these `.npy` files and converting them into PyTorch tensors for the model. You may need to adapt these dataset classes if your file naming conventions or internal data structures differ significantly.