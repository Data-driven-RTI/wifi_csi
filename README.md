# Dataset Structure and Loading Guide

This guide outlines the directory structure and data format for the provided wireless signal segmentation dataset. It also explains how the accompanying `train_evaluate_combined.py` script loads this data.

## 1. Dataset Directory Structure

The dataset is organized as follows. Please place the downloaded data maintaining this structure:

# dataset_root/
# ├── processed_data_csi/ # Contains Channel State Information (CSI) data 
# │ ├── sample_001_features.npy # Feature file for sample 1
# │ ├── sample_001_gt.npy # Ground truth mask for sample 1
# │ ├── sample_002_features.npy
# │ ├── sample_002_gt.npy
# │ └── ... # More CSI feature and ground truth pairs
# │
# ├── processed_data_rssi/ # Contains Received Signal Strength Indicator (RSSI) data (if applicable)
# │ ├── item_A_features.npy # Feature file for item A
# │ ├── item_A_gt.npy # Ground truth mask for item A
# │ └── ... # More RSSI feature and ground truth pairs
