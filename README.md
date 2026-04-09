# groupwork
## VO

## UNet
UNet for Aerial Image Semantic Segmentation
This project implements a U-Net model for semantic segmentation on the Amtown aerial dataset. Through careful parameter tuning and training pipeline design, the model achieves stable and reliable segmentation performance with clear improvements on key metrics.
1. Project Information
Task: Semantic Segmentation
Dataset: Amtown aerial imagery (26 classes)
Framework: PyTorch
Environment: Google Colab (GPU)
Training Epochs: 2
Image Scale: 0.5
Batch Size: 2
2. Key Parameter Tuning and Motivation
2.1 Loss Function Weight Tuning
Initial Setup: Cross Entropy + Dice Loss (0.5 / 0.5)
Problem: Low mIoU, unstable training, weak small-class segmentation
Tuning: Adjusted to 0.4 CE + 0.6 Dice
Rationale:
Dice loss is more robust to class imbalance, which is critical for small-class learning
Cross Entropy maintains stable classification guidance to prevent training oscillation
The 0.4/0.6 ratio provides the best balance between convergence speed and segmentation quality
Result: mIoU improved from ~0.35 to 0.3778
2.2 Learning Rate Tuning
Initial Setup: 1e-4
Problem: Loss oscillation, difficult to converge to optimal solution
Tuning: Changed to 5e-5
Rationale:
Lower learning rate stabilizes late-stage training
Prevents overshooting optimal weights
Ensures steady, consistent improvement of mIoU
Result: Smooth loss decay, stable validation performance
2.3 Other Important Fixed Parameters
IGNORE_INDEX = 255: Filter empty/unnamed classes in cmap.py to avoid invalid annotation interference
Optimizer: AdamW (weight decay = 1e-8) to prevent overfitting
Gradient Clipping: 1.0 to avoid gradient explosion
Mixed Precision Training: Enabled for faster training and reduced memory usage
3. Final Model Performance (Epochs 1–2)
表格
Metric	Value
mean_iou	0.3778
mean_dice	0.4169
pixel_accuracy	0.9112
mean_accuracy	0.4214
Evaluated images	1380
Number of classes	26
Key Class Performance
表格
Class ID	Class Name	IoU	Dice
0	background	0.6704	0.8026
1	roof	0.8564	0.9226
3	paved_motor_road	0.8488	0.9182
13	green_field	0.9180	0.9572
14	wild_field	0.8148	0.8979
19	paved_walk	0.8709	0.9310
4. File Structure (Unet Folder)
plaintext
Unet/
├── training_report.json       # GitHub-compliant training report with full metrics and class mapping
├── amtown02_evaluation_report.json  # Raw evaluation report with per-class IoU/accuracy
├── checkpoint_epoch1.pth      # Model weights after Epoch 1
├── checkpoint_epoch2.pth      # Optimal model weights after Epoch 2
├── 0.4_0.6_5e_Step15.png      # Training process visualization
└── 0.4_0.6_5e_Step20.png      # Segmentation result visualization
5. Training & Reproduction Guide
5.1 Environment Setup
Framework: PyTorch
Dependencies: torch, torchvision, numpy, tqdm, PIL
Runtime: Google Colab (GPU acceleration)
5.2 Core Training Hyperparameters (Ready-to-Use)
python
运行
### Fixed optimal parameters
ce_weight = 0.4
dloss_weight = 0.6
learning_rate = 5e-5
SCALE = 0.5
BATCH_SIZE = 2
EPOCHS = 2
IGNORE_INDEX = 255
optimizer = AdamW(weight_decay=1e-8)
6. Future Improvement Directions
Extend training epochs to further boost mIoU beyond 0.4
Implement data augmentation (random cropping, flipping) to improve small-class performance
Explore advanced U-Net variants (e.g., Attention U-Net) for enhanced segmentation accuracy
Complete class frequency statistics to enrich the per_class_results field in the training report
## 3D Reconstruction
