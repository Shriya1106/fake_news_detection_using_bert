# Fake News Detection with BERT

A complete end-to-end fake news detection system using fine-tuned BERT/DistilBERT models.

## Quick Start

### 1. Install Dependencies
```bash
pip install transformers datasets accelerate gradio scikit-learn seaborn matplotlib pandas torch
```

### 2. Get a Real Dataset

The code will automatically try to load datasets in this order:

#### Option A: Download from Kaggle (Recommended)
1. **Fake News Detection Dataset** (44,000+ samples)
   ```bash
   # Install Kaggle CLI
   pip install kaggle
   
   # Setup Kaggle API credentials (get from kaggle.com/account)
   # Place kaggle.json in ~/.kaggle/
   
   # Download dataset
   kaggle datasets download -d clmentbisaillon/fake-and-real-news-dataset
   unzip fake-and-real-news-dataset.zip
   ```

2. **WELFake Dataset** (72,000+ samples)
   ```bash
   kaggle datasets download -d saurabhshahane/fake-news-classification
   unzip fake-news-classification.zip
   ```

#### Option B: Use HuggingFace Datasets (Automatic)
The code will automatically try to download from HuggingFace if no local file is found:
```python
# This happens automatically in the code
from datasets import load_dataset
dataset = load_dataset('GonzaloA/fake_news')
```

#### Option C: Use Your Own CSV
Place any CSV file with these columns in the project directory:
- A text column (named: `text`, `title`, `content`, etc.)
- A label column (named: `label`, `class`, etc.)
- Labels should be: `0/1`, `Real/Fake`, or `REAL/FAKE`

Supported filenames: `fake_news.csv`, `news.csv`, `WELFake_Dataset.csv`, `train.csv`

### 3. Run Training
```bash
python fake_news_detection_bert_mini_project\ \(1\).py
```

## Expected Performance

With proper datasets (10,000+ samples):
- **Accuracy**: 92-98%
- **F1-Score**: 0.90-0.97
- **Training time**: 30-60 minutes (with GPU)

With demo dataset (30 samples):
- **Accuracy**: 60-75% (not reliable)
- Use only for testing code functionality

## Configuration

Edit the `CONFIG` dictionary to customize:

```python
CONFIG = {
    'model_name': 'distilbert-base-uncased',  # or 'bert-base-uncased', 'roberta-base'
    'max_length': 512,        # Token limit (reduce if OOM)
    'batch_size': 16,         # Reduce to 8 if out of memory
    'epochs': 5,              # Training epochs
    'lr': 2e-5,              # Learning rate
    'early_stopping_patience': 3,  # Stop if no improvement
}
```

## Model Options

Try different models for better accuracy:

| Model | Size | Speed | Accuracy |
|-------|------|-------|----------|
| `distilbert-base-uncased` | 66M | Fast | Good |
| `bert-base-uncased` | 110M | Medium | Better |
| `roberta-base` | 125M | Slower | Best |

## Improving Accuracy

1. **Use more data**: 10,000+ samples minimum
2. **Increase epochs**: Try 5-10 epochs
3. **Larger model**: Use `bert-base-uncased` or `roberta-base`
4. **Longer sequences**: Increase `max_length` to 512
5. **Data augmentation**: Add paraphrasing or back-translation
6. **Ensemble**: Combine multiple models

## Troubleshooting

### Out of Memory (OOM)
```python
CONFIG['batch_size'] = 8  # Reduce batch size
CONFIG['max_length'] = 256  # Reduce sequence length
```

### Low Accuracy (<70%)
- Use a real dataset (not demo data)
- Train for more epochs (5-10)
- Check data quality and balance
- Try a larger model

### Slow Training
- Use GPU (Google Colab free tier works)
- Use DistilBERT instead of BERT
- Reduce `max_length`

## Project Structure

```
.
├── fake_news_detection_bert_mini_project (1).py  # Main script
├── README.md                                      # This file
├── models/
│   └── best_model/                               # Saved model
└── [your_dataset].csv                            # Dataset file
```

## Deployment

The script includes a Gradio interface for easy testing:
```python
# Automatically launches at the end of training
demo.launch(share=True)  # Creates public URL
```

## Resources

- [Kaggle Fake News Datasets](https://www.kaggle.com/search?q=fake+news)
- [HuggingFace Transformers](https://huggingface.co/docs/transformers)
- [BERT Paper](https://arxiv.org/abs/1810.04805)

## Contributing

Feel free to improve the model or add new features!

## License

MIT License - feel free to use for your projects.
