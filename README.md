
Where:

- `L_seg`: CrossEntropyLoss (segmentation)
- `L_cls`: CrossEntropyLoss (classification)

---

# Training Details

- Optimizer: Adam
- Learning rate: 1e-4
- Batch size: 8
- Epochs: 10
- Loss: multi-task loss with classification task weighting
- Device: CPU / CUDA (auto-detect)

---

# Evaluation Metrics

## Classification

- Accuracy
- Precision (weighted)
- Recall (weighted)
- F1-score (weighted)

## Segmentation

- Dice score (foreground classes only, excluding background)

---

# Final Results

## Classification Performance

- Accuracy:  **0.8481**
- Precision: **0.8816**
- Recall:    **0.8481**
- F1-score:  **0.8554**

## Segmentation Performance

- Dice Score: **0.9171**

---

# Classification Report

```text
              precision    recall  f1-score   support

  Lymphocyte       1.00      0.88      0.94        41
    Monocyte       0.87      0.72      0.79        18
  Neutrophil       0.60      0.92      0.73        13
  Eosinophil       0.75      0.86      0.80         7

    accuracy                           0.85        79
   macro avg       0.80      0.85      0.81        79
weighted avg       0.88      0.85      0.86        79
```

---

# Segmentation Encoding

Mask pixel values:

- `0`   → Background
- `128` → Nucleus
- `255` → Cytoplasm

---

# Visualization

The framework supports:

## Segmentation visualization

- Input image
- Ground truth mask
- Predicted mask

## Classification visualization

- Correct predictions
- Incorrect predictions

## Grad-CAM explainability

Highlights regions influencing classification decisions.

---

# Grad-CAM

Grad-CAM is applied on:

- Bottleneck layer (`model.center`)

> Important: Grad-CAM is computed on the bottleneck convolutional block, not encoder/decoder layers.

It produces heatmaps showing which regions of the cell most influence classification decisions.

---

# Checkpoints

Pretrained model weights are not included in this repository due to file size limitations.

## Download Pretrained Model

Download the checkpoint from Google Drive:

https://drive.google.com/file/d/1BCZJtTNnL3xxWzJjYKjZZIO9T2th2zUd/view?usp=sharing

The checkpoint is provided for reproducibility and demonstration purposes only.

After downloading, place the file in the following directory:

```text
models/checkpoints/wbc_multitask_checkpoint.ckpt
```

## Additional Information

For more details about loading the checkpoint, inference, and full usage instructions, see:

```text
models/checkpoints/README.md
```
---

# Inference Pipeline

At inference time:

1. Load model checkpoint
2. Forward pass returns:
   - segmentation map
   - classification logits
3. Apply argmax for predictions
4. Optional Grad-CAM visualization

---

# Installation

```bash
git clone https://github.com/SarvinChitsaz/WBC-MTL.git

cd WBC-MTL

pip install -r requirements.txt
```
---

# Requirements

```text
Python >= 3.9
torch >= 2.0
torchvision
albumentations
scikit-learn
opencv-python
matplotlib
numpy
pandas
```
---
# Visualization Results

## Segmentation Samples

<p align="center">
  <img src="assets/results/segmentation/seg_sample_1.png" width="500">
</p>

<p align="center">
  <img src="assets/results/segmentation/seg_sample_2.png" width="500">
</p>

_Side-by-side comparison of original image, ground-truth mask, and predicted mask._

---

## Classification Predictions

<p align="center">
  <img src="assets/results/classification/classification_samples.png" width="700">
</p>

_Correctly and incorrectly classified white blood cell samples._

---

## Confusion Matrix

<p align="center">
  <img src="assets/results/confusion_matrix/confusion_matrix.png" width="500">
</p>

_Per-class classification performance across all four WBC categories._

---

## Grad-CAM Explainability

<p align="center">
  <img src="assets/results/gradcam/gradcam_sample_1.png" width="500">
</p>

<p align="center">
  <img src="assets/results/gradcam/gradcam_sample_2.png" width="500">
</p>

_Grad-CAM heatmaps providing interpretability for classification predictions._

---

# Notes

- Dataset must be downloaded manually from the official repository
- Images must contain both `.bmp` image and `.png` mask
- Classes with fewer than 20 samples are removed automatically
- Model supports CPU and GPU execution
- Grad-CAM uses forward/backward hooks on the bottleneck layer (`model.center`)

---
# Limitations

- This project is a research/prototype implementation and is not intended for clinical diagnosis.
- Evaluation is performed on a relatively small public dataset.
- No external validation dataset is used.
- The current implementation uses a train/test split only.
- Grad-CAM is used as an interpretability aid and should not be considered clinical evidence.

---

# License

MIT
