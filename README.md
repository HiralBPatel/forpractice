<div align="center">

# 🌿 Crop Leaf Disease Classification
### Deep Learning Research Project using CNN & Transfer Learning

<img src="https://img.shields.io/badge/TensorFlow-2.x-orange?logo=tensorflow">
<img src="https://img.shields.io/badge/Python-3.10-blue?logo=python">
<img src="https://img.shields.io/badge/Keras-DeepLearning-red?logo=keras">
<img src="https://img.shields.io/badge/Status-Research-success">

---

### 🚀 Multi-Crop Leaf Disease Classification System

Classifying diseases in:

🌱 Papaya • 🌿 Guava • 🍆 Brinjal • 🌾 Cumin • 🌴 Castor

Using:
- CNN
- MobileNetV2
- EfficientNetB0
- ResNet50

</div>

---

# 📖 Project Overview

This project implements an end-to-end deep learning pipeline for automatic crop leaf disease classification using TensorFlow/Keras.

## ✨ Features

- ✅ Custom CNN Baseline
- ✅ Transfer Learning
- ✅ EfficientNetB0
- ✅ ResNet50
- ✅ MobileNetV2
- ✅ TensorFlow Lite Export
- ✅ Grad-CAM Explainability
- ✅ Data Augmentation
- ✅ Evaluation Metrics
- ✅ Confusion Matrix Visualization

---

# 🧠 Models Used

| Model | Size | Speed | Accuracy | Use Case |
|---|---|---|---|---|
| CNN Baseline | Small | Fast | Medium | Research baseline |
| MobileNetV2 | 14 MB | ⚡ Very Fast | -- | Mobile deployment |
| EfficientNetB0 | 29 MB | 🚀 Medium | -- | Production |
| ResNet50 | 98 MB | 🐢 Slower | -- | High accuracy |

---

# 📂 Project Structure

```bash
cnn_model/
│
├── notebooks/
│   ├── 01_data_check.ipynb
│   ├── 02_dataset_visualization.ipynb
│   ├── 03_data_preprocessing.ipynb
│   ├── 04_cnn_baseline.ipynb
│   ├── 05_mobilenet_training.ipynb
│   ├── 06_model_evaluation.ipynb
│   ├── 07_prediction_testing.ipynb
│   ├── 08_tflite_conversion.ipynb
│   └── 09_gradcam_explainability.ipynb
│
├── models/
├── utils/
├── results/
└── README.md
