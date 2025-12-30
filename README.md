# Brain Tumour Classification: MIGT vs Random

This repository presents a brain tumor classification framework based on an Xception CNN and compares two dataset partitioning strategies:
- Random split (baseline)
- Mutual Information Guided Training (MIGT)

The goal is to evaluate the effect of information-aware dataset splitting on training stability and model generalization.

---

## Dataset
- Classes: Brain Tumour, Healthy
- Image size: 224 × 224
- Total samples: ~4,500 MRI images
- Split ratio:
  - Train: 50%
  - Validation: 10%
  - Test: 40%

Two versions of the dataset are provided:
- MIGT split
- Random split

---

## Mutual Information Guided Training (MIGT)
MIGT partitions the dataset based on the mutual information (MI) between images and a reference image within each class.  
Images are sorted by their MI values and grouped into multiple information bins.  
Each bin contributes proportionally to the training, validation, and test sets, ensuring that different levels of visual information complexity are represented across all splits.

This strategy reduces sampling bias and improves training stability compared to purely random splitting.

---

## Baseline: Random Split
The random split assigns samples to training, validation, and test sets without considering image information content.  
It is used as a baseline for fair comparison with MIGT.

---

## Model Architecture
The classification model is based on **Xception**, pre-trained on ImageNet.

Architecture details:
- Input size: 224 × 224 × 3
- Backbone: Xception (ImageNet weights)
- Global Average Pooling
- Dropout
- Fully connected layer with sigmoid activation (binary classification)

The Xception backbone is initially frozen for feature extraction and later partially unfrozen during fine-tuning.

---

## Training Strategy
Training is performed in two stages:

### Stage 1: Feature Extraction
- Xception backbone frozen
- Only classification head trained
- Optimizer: Adam
- Loss function: Binary Cross-Entropy
- Metrics: Accuracy, AUC

### Stage 2: Fine-Tuning
- Upper layers of Xception unfrozen
- Reduced learning rate
- Improves task-specific feature adaptation

Data augmentation is applied during training to improve robustness and reduce overfitting.

The best model is selected based on minimum validation loss.

---

## Evaluation
Model performance is evaluated using:
- Accuracy
- AUC (Area Under the ROC Curve)
- Precision, Recall, and F1-score

The same model architecture, training configuration, and hyperparameters are used for both MIGT and Random splits to ensure a fair comparison.

---

## Results

| Method | Test Accuracy | Test AUC | Macro F1 |
|-------|---------------|----------|----------|
| MIGT  | **97.61%**    | **0.9939** | **0.98** |
| Random| 97.57%        | 0.9936   | 0.98 |

Although both methods achieve similar final accuracy on this dataset, MIGT demonstrates more stable training–validation convergence and a principled, reproducible dataset partitioning strategy.

---

## Repository Structure
```bash
dataset/
├── MIGT/
│   ├── train/
│   ├── val/
│   └── test/
├── Random/
│   ├── train/
│   ├── val/
│   └── test/

models/
├── MIGT/
│   ├── xception_best_model.h5
│   └── xception_full_model.h5
├── Random/
│   ├── xception_best_model.h5
│   └── xception_full_model.h5
