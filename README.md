# groupwork
## VO

## UNet
# groupwork
## VO
### AAE5303 Assignment: Visual Odometry with ORB-SLAM3
This repository contains the implementation and analysis of monocular visual odometry using the ORB-SLAM3 framework on the HKisland_GNSS03 UAV aerial imagery dataset.

---

### Key Results
| Metric | Value | Description |
|--------|-------|-------------|
| ATE RMSE | 1.373 m | Global accuracy after Sim(3) alignment |
| RPE Trans Drift | 4.02 m/m | Translation drift rate (delta=10 m) |
| RPE Rot Drift | 147.83 deg/100m | Rotation drift rate (delta=10 m) |
| Completeness | 4.81% | Matched poses / total ground-truth poses |

---

### Introduction
ORB-SLAM3 is a state-of-the-art visual SLAM system capable of monocular, stereo, and visual-inertial odometry.

This assignment focuses on Monocular VO mode, which:
- Uses only camera images for pose estimation
- Enables 6-DoF trajectory tracking without GPS/IMU
- Is robust to dynamic objects and illumination changes

---

## UNet
### AAE5303 Assignment: U-Net for Aerial Image Semantic Segmentation
This project implements a U-Net model for semantic segmentation on the Amtown aerial imagery dataset, with systematic hyperparameter tuning to achieve stable segmentation performance and clear metric improvements.

---

### Key Results
| Metric | Value | Description |
|--------|-------|-------------|
| mean_iou | 0.3778 | Mean Intersection over Union (primary segmentation accuracy metric) |
| mean_dice | 0.4169 | Mean Dice Coefficient (segmentation region completeness) |
| pixel_accuracy | 0.9112 | Pixel Accuracy (large-class segmentation & visual quality) |
| mean_accuracy | 0.4214 | Mean Accuracy (average performance across all 26 classes) |
| Evaluated Images | 1380 | Full dataset coverage (train/val/test split) |

---

### Key Parameter Tuning & Rationale
#### 1. Loss Function Weighting
- **Initial Setup**: Cross-Entropy (CE) + Dice Loss (0.5 / 0.5)
- **Problem**: Low mIoU (~0.35), unstable training, poor small-class segmentation due to severe class imbalance
- **Tuned Setup**: **0.4 CE + 0.6 Dice**
- **Rationale**:
  - Dice loss is more robust to class imbalance, critical for small-class learning
  - CE loss provides stable classification supervision to prevent training oscillation
  - 0.4/0.6 ratio balances large-class accuracy and small-class performance
- **Result**: mIoU improved from ~0.35 to 0.3778, mean Dice stabilized at 0.4169

#### 2. Learning Rate
- **Initial Setup**: `1e-4`
- **Problem**: Loss oscillation in late training, failure to converge to global optimum
- **Tuned Setup**: **`5e-5`**
- **Rationale**:
  - High learning rate causes overshooting optimal weights, leading to unstable training
  - Lower learning rate stabilizes training and ensures steady mIoU improvement
- **Result**: Smooth loss decay, stable validation performance, continuous mIoU growth

#### 3. Fixed Critical Parameters
- `IGNORE_INDEX = 255`: Filters empty/unnamed classes in `cmap.py` to avoid invalid annotation interference
- **Optimizer**: `AdamW` (weight decay = `1e-8`) to prevent overfitting
- **Image Scale**: `0.5` (balances GPU memory usage and input resolution)
- **Batch Size**: `2` (adapted to Colab GPU memory constraints)
- **Training Epochs**: `2`

---

### File Structure (Unet Folder)
```
Unet/
├── training_report.json       # GitHub-compliant training report with full metrics & class mapping
├── amtown02_evaluation_report.json  # Raw evaluation report with per-class IoU/accuracy
├── checkpoint_epoch1.pth      # Model weights after Epoch 1
├── checkpoint_epoch2.pth      # Optimal model weights after Epoch 2
├── 0.4_0.6_5e_Step15.png      # Training process visualization
└── 0.4_0.6_5e_Step20.png      # Segmentation result visualization
```

---

### Introduction
U-Net is a widely adopted encoder-decoder architecture for semantic segmentation, designed specifically for medical and aerial imagery tasks.

This assignment focuses on aerial image semantic segmentation, which:
- Classifies each pixel into 26 distinct land-use categories
- Enables automated mapping and analysis of urban/rural environments
- Supports downstream tasks like UAV navigation and urban planning

---

## 3D Reconstruction
*(To be completed by the 3D Reconstruction module team)*


## 3D Reconstruction
