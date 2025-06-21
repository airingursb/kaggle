# 🚀 Kaggle LLM Classification Fine-tuning Solution

## 📋 Project Overview

This project provides a comprehensive solution for the **Kaggle LLM Classification Fine-tuning Competition**. The task is to predict which of two LLM responses is better for a given prompt, or if they are tied in quality.

## 🎯 Problem Description

- **Input**: Prompt + two model responses (response_a, response_b)
- **Output**: 3-class probability distribution (Model A wins, Model B wins, Tie)
- **Evaluation**: Multi-class Log Loss
- **Goal**: Accurately rank LLM response quality

## 🏗️ Solution Architecture

### 1. 📊 Data Processing
- Clean JSON-formatted text data
- Concatenate prompt and responses: `prompt [SEP] response_a [SEP] response_b`
- Stratified train/validation split maintaining class balance
- Optional data sampling for faster training

### 2. 🤖 Model Architecture
- **Base Model**: DistilBERT-base-uncased (optimized for speed)
- **Fallbacks**: BERT-base, RoBERTa-base, DeBERTa-v3-base
- **Input Format**: `[CLS] prompt [SEP] response_a [SEP] response_b [SEP]`
- **Output**: 3-class softmax probability distribution
- **Classification Head**: Linear layer with dropout

### 3. ⚡ Training Strategy (Speed-Optimized)
- **Loss Function**: KL Divergence Loss
- **Optimizer**: AdamW with optimized parameters
- **Scheduler**: Linear warmup + linear decay
- **Mixed Precision**: Automatic Mixed Precision (AMP) for 2x speedup
- **Batch Size**: 16 (increased for better GPU utilization)
- **Sequence Length**: 256 tokens (reduced from 512 for speed)
- **Epochs**: 2 (reduced from 3 for faster training)

## 🚀 Quick Start

### For Kaggle Users:
1. **Upload the notebook**: `llm_classification_finetuning.ipynb`
2. **Enable internet**: Settings → Internet → ON
3. **Enable GPU**: Settings → Accelerator → GPU T4 x2
4. **Run all cells**: The notebook handles everything automatically

### For Local Development:
```bash
# Clone repository
git clone <repository-url>
cd llm_classification_finetuning

# Install dependencies
pip install -r requirements.txt

# Run Jupyter notebook
jupyter notebook llm_classification_finetuning.ipynb
```

## 📁 File Structure
```
📦 llm_classification_finetuning/
├── 📓 llm_classification_finetuning.ipynb  # Main solution notebook
├── 📋 requirements.txt                     # Python dependencies
├── 📖 README.md                           # This file
├── 🐍 llm_classification_solution.py      # Legacy Python script
├── 🔧 simple_baseline.py                  # Simple baseline
├── ⚡ optimized_solution.py               # Optimized version
└── 🚫 .gitignore                          # Git ignore file
```

## ⚡ Speed Optimizations

The notebook is heavily optimized to reduce training time from **55+ hours to 2-6 hours**:

### 🏃‍♂️ Key Optimizations:
1. **Model Selection**: Prioritize DistilBERT (3-4x faster than BERT)
2. **Sequence Length**: 256 tokens (4x faster than 512)
3. **Mixed Precision**: AMP for 2x speed boost
4. **Data Sampling**: Use 80% of data with stratified sampling
5. **Batch Optimization**: Larger batches + optimized DataLoader
6. **Reduced Epochs**: 2 epochs instead of 3
7. **Smart Validation**: Less frequent validation for speed

### 🚀 Ultra-Fast Mode:
For maximum speed (under 30 minutes), uncomment the ULTRA_FAST_MODE in the notebook:
- Uses 20% of data
- Single epoch
- 128 token sequences
- 32 batch size

## 📊 Performance Features

### 🎯 Accuracy Features:
- Stratified sampling maintains class balance
- KL Divergence loss for probability distribution learning
- Gradient clipping prevents training instability
- Best model checkpointing
- Comprehensive validation metrics

### 🔧 Robustness Features:
- Automatic model fallback (works without internet)
- Error handling for Kaggle environment
- Memory optimization and cleanup
- Progress tracking with time estimates

## 🛠️ Configuration Options

The notebook provides flexible configuration:

```python
# Speed vs Accuracy tradeoffs
MODEL_NAME = 'distilbert-base-uncased'  # Fast
MAX_LENGTH = 256                        # Balanced
BATCH_SIZE = 16                         # GPU optimized
NUM_EPOCHS = 2                          # Fast training
SAMPLE_FRACTION = 0.8                   # Use 80% of data
USE_MIXED_PRECISION = True              # 2x speedup
```

## 🎯 Expected Results

- **Training Time**: 2-6 hours (vs original 55+ hours)
- **Validation Log Loss**: ~0.6-0.8 (competitive baseline)
- **Model Size**: ~66M parameters (DistilBERT)
- **Memory Usage**: ~4-6GB GPU memory

## 🚨 Kaggle Setup Notes

**Common Issues & Solutions:**
- **ConnectionError**: Enable internet in notebook settings
- **CUDA OOM**: Reduce batch size to 8 or 4
- **Missing packages**: The notebook auto-installs missing dependencies
- **Model loading fails**: Uses automatic fallback to smaller models

## 💡 Tips for Better Performance

1. **For Higher Accuracy**: Use DeBERTa-v3-base (if time allows)
2. **For Faster Training**: Enable ULTRA_FAST_MODE
3. **For Memory Issues**: Reduce batch_size and max_length
4. **For Kaggle**: Enable internet + GPU T4 x2

## 🤝 Contributing

Feel free to submit issues, suggestions, or improvements to make this solution even better!

## 📄 License

This project is open source and available under the MIT License.