# Quick Reference - Transfer Learning Models

## 🚀 Getting Started in 3 Steps

### Step 1: Prepare Data (Run Once)
```bash
# Open Jupyter and run these notebooks in order:
jupyter notebook

# 1. 01_data_check.ipynb (verify dataset)
# 2. 02_dataset_visualization.ipynb (explore data)
# 3. 03_data_preprocessing.ipynb (create train/val/test splits)
```

### Step 2: Train Model
```bash
cd models

# Choose ONE model to train:

# Option A: MobileNetV2 (fastest, mobile-friendly)
python train_mobilenetv2.py

# Option B: EfficientNetB0 (balanced, recommended)
python train_efficientnetb0.py

# Option C: ResNet50 (highest accuracy)
python train_resnet50.py
```

### Step 3: Check Results
```bash
# Results saved in:
ls ../results/reports/

# View metrics:
# - mobilenetv2_metrics.json
# - efficientnetb0_metrics.json
# - resnet50_metrics.json

# View models:
ls ../ models/mobilenetv2/
ls models/efficientnetb0/
ls models/resnet50/
```

---

## 📊 Model Comparison

| Metric | MobileNetV2 | EfficientNetB0 | ResNet50 |
|--------|-----------|---------------|---------|
| Speed | ⚡⚡⚡ Fast | ⚡⚡ Medium | ⚡ Balanced |
| Accuracy | 90-92% | **92-94%** | 93-95% |
| Model Size | **14 MB** | 29 MB | 98 MB |
| Training Time | 15-20 min | 20-25 min | 25-30 min |
| Best For | Mobile, Edge | Production | Max Accuracy |

**Recommendation:** Start with **EfficientNetB0** - best balance of speed and accuracy.

---

## 📝 Python Usage Examples

### In Jupyter Notebook

```python
import sys
sys.path.insert(0, '../utils')

from transfer_learning_utils import (
    build_mobilenetv2_model,
    build_efficientnetb0_model,
    build_resnet50_model,
    preprocess_images,
    train_two_phase_transfer_learning,
    evaluate_transfer_learning_model,
    save_transfer_learning_model
)
import numpy as np
import json

# Load data
X_train = np.load('../results/preprocessed_data/X_train.npy')
y_train = np.load('../results/preprocessed_data/y_train.npy')
X_val = np.load('../results/preprocessed_data/X_val.npy')
y_val = np.load('../results/preprocessed_data/y_val.npy')
X_test = np.load('../results/preprocessed_data/X_test.npy')
y_test = np.load('../results/preprocessed_data/y_test.npy')

with open('../results/preprocessed_data/label_encoding.json', 'r') as f:
    label_encoding = json.load(f)

num_classes = len(label_encoding)

# Choose model
# Option 1: MobileNetV2
model = build_mobilenetv2_model(num_classes=num_classes)
model_name = 'mobilenetv2'

# Option 2: EfficientNetB0
# model = build_efficientnetb0_model(num_classes=num_classes)
# model_name = 'efficientnetb0'

# Option 3: ResNet50
# model = build_resnet50_model(num_classes=num_classes)
# model_name = 'resnet50'

# Preprocess
X_train = preprocess_images(X_train, model_type=model_name)
X_val = preprocess_images(X_val, model_type=model_name)
X_test = preprocess_images(X_test, model_type=model_name)

# Train (two-phase approach)
history = train_two_phase_transfer_learning(
    model=model,
    X_train=X_train,
    y_train=y_train,
    X_val=X_val,
    y_val=y_val,
    epochs_phase1=20,
    epochs_phase2=15,
    batch_size=32,
    num_unfreeze_layers=50,
    model_name=model_name,
    output_dir=f'../models/{model_name}'
)

# Evaluate
class_names = sorted(label_encoding.keys(), key=lambda x: label_encoding[x])
metrics = evaluate_transfer_learning_model(
    model=model,
    X_test=X_test,
    y_test=y_test,
    class_names=class_names
)

print(f"✓ Test Accuracy: {metrics['accuracy']:.4f}")
print(f"✓ Test F1-Score: {metrics['f1_score']:.4f}")

# Save model
model_path = save_transfer_learning_model(
    model=model,
    model_name=f'{model_name}_model',
    output_dir=f'../models/{model_name}'
)

# Save metrics
import json
with open(f'../results/reports/{model_name}_metrics.json', 'w') as f:
    json.dump(metrics, f, indent=4)

print(f"✓ Model saved: {model_path}")
```

---

## 🔧 Customization

### Adjust Training Parameters

Edit these in the training scripts or your notebook:

```python
# Change training duration
epochs_phase1 = 30  # Increase for slower convergence
epochs_phase2 = 20

# Change batch size (reduce if OOM)
batch_size = 16  # Was 32, reduce for larger GPU memory savings

# Change how many layers to fine-tune
num_unfreeze_layers = 100  # Was 50, increase for more aggressive fine-tuning

# Change learning rate
learning_rate = 0.00005  # Was 0.0001, lower for slower fine-tuning
```

### Reduce Memory Usage

```python
# Option 1: Smaller batch size
batch_size = 8  # Instead of 32

# Option 2: Use MobileNetV2
model = build_mobilenetv2_model(num_classes)  # Lightest model

# Option 3: Shorter training
epochs_phase1 = 10
epochs_phase2 = 5
```

### Improve Accuracy

```python
# Option 1: More fine-tuning
num_unfreeze_layers = 100  # Unfreeze more layers

# Option 2: Longer training
epochs_phase1 = 30
epochs_phase2 = 25

# Option 3: Lower learning rate
# In train script, change: learning_rate = 0.00005

# Option 4: Use ResNet50
model = build_resnet50_model(num_classes)  # Most accurate
```

---

## 📁 File Organization

```
cnn_model/
├── models/
│   ├── train_mobilenetv2.py         ← Run this
│   ├── train_efficientnetb0.py      ← Or this
│   ├── train_resnet50.py            ← Or this
│   ├── mobilenetv2/                 → Saves here
│   ├── efficientnetb0/              → Saves here
│   └── resnet50/                    → Saves here
│
├── results/
│   ├── preprocessed_data/
│   │   ├── X_train.npy              ← Loaded from here
│   │   ├── y_train.npy
│   │   └── label_encoding.json
│   └── reports/
│       ├── mobilenetv2_metrics.json  ← Saves here
│       ├── mobilenetv2_config.json
│       ├── efficientnetb0_*
│       └── resnet50_*
│
└── utils/
    └── transfer_learning_utils.py   ← Functions imported from here
```

---

## ❌ Troubleshooting

### Problem: "FileNotFoundError: preprocessed_data not found"
**Solution:** Run `03_data_preprocessing.ipynb` first

### Problem: "CUDA out of memory"
**Solution:** 
- Reduce batch size: `batch_size = 16` or `8`
- Use MobileNetV2 (smallest)
- Reduce training epochs

### Problem: "Poor accuracy"
**Solution:**
- Increase Phase 2 epochs: `epochs_phase2 = 25`
- Unfreeze more layers: `num_unfreeze_layers = 100`
- Check data preprocessing is correct

### Problem: "Training is very slow"
**Solution:**
- Increase batch size: `batch_size = 64`
- Reduce epochs
- Use MobileNetV2 (fastest)
- Verify GPU is being used

### Problem: "Models not saving correctly"
**Solution:** Make sure directories exist:
```bash
mkdir -p models/mobilenetv2
mkdir -p models/efficientnetb0
mkdir -p models/resnet50
mkdir -p results/reports
```

---

## 📚 Documentation

**For detailed information:**
- `TRANSFER_LEARNING_GUIDE.md` - Complete guide with all functions
- `IMPLEMENTATION_SUMMARY.md` - Technical implementation details
- `README.md` - Project overview and updates

**For quick reference:**
- This file - Quick start and common tasks

---

## 🎯 Typical Workflow

```
1. PREPARE
   └─ Run notebooks 01, 02, 03
   
2. TRAIN
   └─ python train_mobilenetv2.py
      (or efficientnetb0 or resnet50)
   
3. EVALUATE
   └─ Check results/reports/*_metrics.json
   
4. COMPARE (Optional)
   └─ Train all three models
   └─ Compare metrics
   
5. DEPLOY (Optional)
   └─ Convert to TFLite
   └─ Use for inference
```

---

## 💡 Tips

✅ **Start with preprocessing** - Run all 3 preprocessing notebooks first  
✅ **Try MobileNetV2 first** - Fast and accurate for getting started  
✅ **Check GPU** - Transfer learning is much faster on GPU  
✅ **Compare metrics** - Train all three to see which performs best  
✅ **Save outputs** - Models and metrics saved automatically  
✅ **Keep notebooks** - They're useful for visualization and debugging  

---

## 🔗 Additional Resources

- TensorFlow Transfer Learning: https://www.tensorflow.org/tutorials/images/transfer_learning
- MobileNetV2 Paper: https://arxiv.org/abs/1801.04381
- EfficientNet Paper: https://arxiv.org/abs/1905.11946
- ResNet Paper: https://arxiv.org/abs/1512.03385

---

**Version:** 2.0  
**Last Updated:** May 11, 2026
