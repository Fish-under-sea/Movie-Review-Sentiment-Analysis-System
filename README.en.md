> **🗄️ Project completed** · Last commit: 2025-12-24 (about 9 months ago)
>
> A course final project, now that the course has ended. It is functionally complete and the code is kept for reference; **no further feature work or bug fixes are accepted**.

<div align="center">

# 🎬 Movie Review Sentiment Analysis System

**BERT-based Chinese three-class sentiment analysis — a complete pipeline from data processing to a web demo**

A *Final Project* course assignment · code and results kept for reference

<sub>Dataset: 798 movie review samples · three classes (positive / negative / neutral)</sub>

[简体中文](README.md) · **English**

</div>

---

## 📚 Overview

A Chinese movie review sentiment classification system built on a pretrained language model (**BERT**), making **three-class** sentiment judgements on review text: **positive / negative / neutral**.

The project covers a complete machine learning pipeline:

```
Data preparation  →  Model fine-tuning  →  Evaluation  →  Interactive demo
     │                    │                    │                 │
     ▼                    ▼                    ▼                 ▼
Corpus cleaning      BERT fine-tuning     Accuracy /        Command line
Label mapping        Tunable hyperparams  confusion matrix  + web page
Dataset splitting                         Error analysis    Dual-form demo
```

> **Shared lineage**: this repository and [`Campus-Text-Sentiment-Classification-System`](https://github.com/Fish-under-sea/Campus-Text-Sentiment-Classification-System) share the same code skeleton (the file structure is identical); the only difference is that **the corpus and model artifacts have been swapped**.

## ✨ Key features

- **Complete workflow** — data preparation → model training → evaluation → interactive demo, all in one command
- **Improved data handling** — smart data splitting and data quality checks
- **Detailed evaluation report** — accuracy, classification report, confusion matrix, and error case analysis
- **Dual-form demo** — command-line interaction plus a Flask web version
- **Flexible configuration** — overridable through both command-line arguments and a configuration file
- **Multiple model options** — `bert` / `qwen` / `simple` modes

## ⚠️ Important note on the evaluation results

> **This is the part of the README that most deserves your attention. The two result files in the repository contradict each other — please do not take "100% accuracy" as this project's real performance.**

### The two results in the repository

**① `results/evaluation_results.json` (produced by the evaluation script)**

| Metric | Value |
|--------|-------|
| Accuracy | **100.00%** (121 samples, **0** errors) |
| Per-class F1 | positive 1.0000 / negative 1.0000 / neutral 1.0000 |

**② `results/training_results.json` (produced during training)**

| Metric | Value |
|--------|-------|
| Best validation accuracy | **66.67%** |
| Training samples | **16** |
| Validation samples | **3** |
| Test samples | **4** |
| Epochs / batch size / learning rate | 3 / 4 / 2e-05 |
| Training accuracy curve | 0.375 → 0.6875 → 0.875 |
| Validation accuracy curve | 0.333 → 0.667 → 0.667 |

### Why neither number can be trusted

1. **The sample counts do not match**: the training results record **16 / 3 / 4** samples, while the dataset statistics file (`dataset_stats.json`) shows **558 / 119 / 121**. These are not from the same run — the former is **leftover from an older configuration**.
2. **The test set is far too small**: even taking the 121 samples at face value, **zero errors** on a three-class NLP task is almost certainly not real generalisation ability; it usually points to **data leakage** (test samples appearing in the training set) or an overlap between the evaluation and training sets.
3. **The two contradict each other**: the same model cannot simultaneously have 66.67% validation accuracy and 100% test accuracy.

**Conclusion**: as an **engineering skeleton for a course assignment**, this project is complete and runnable; but the evaluation numbers in the repository **should be read only as proof that the pipeline runs, not as a performance claim**. For real metrics, retrain and re-evaluate following the steps below.

## 🗂️ Dataset

| Item | Value |
|------|-------|
| Valid samples | **798** |
| Splitting method | Random split, ratio **7 : 1.5 : 1.5** |
| Training set | 558 (positive 215 / negative 212 / neutral 131) |
| Validation set | 119 (positive 43 / negative 38 / neutral 38) |
| Test set | 121 (positive 52 / negative 33 / neutral 36) |
| Label mapping | `positive=0` / `negative=1` / `neutral=2` (see `datasets/labels.json`) |

**Class distribution**: positive 310 / negative 283 / neutral 205 — compared with the campus corpus version it shares code with, this dataset has a **more balanced three-class distribution** (the largest-to-smallest ratio is about 1.5:1).

## 🛠️ Tech stack

| Layer | Implementation |
|-------|----------------|
| Language | Python (3.10 recommended) |
| Deep learning | PyTorch + Transformers (`bert-base-chinese`) |
| Data processing | pandas · numpy |
| NLP models | `models/bert-base-chinese/` (pretrained) → `models/finetuned/` (fine-tuned) |
| Web demo | Flask (`app/app.py` + `app/templates/index.html`) |
| Environment management | conda |
| Model distribution | Git LFS (`git lfs pull`) |

## 🚀 Running it

### Environment setup

```bash
# Create and activate the environment
conda create -n movies
conda activate movies

# Install dependencies (using the Tsinghua mirror for speed)
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r req.txt

# Pull the actual model files (the repository stores models with Git LFS)
git lfs pull
```

### Running the code

```bash
cd 'D:\your-file-directory\电影评论情感分析{期末大作业}\'

python .\main.py --all      # Run the complete training pipeline
python .\main.py --demo     # Command-line interactive demo
python .\app\app.py         # Web demo
```

### All available arguments (measured from `main.py` argparse)

| Argument | Purpose |
|----------|---------|
| `--prepare-data` | Data preparation and cleaning |
| `--train` | Training (general entry point) |
| `--train-bert` | Explicitly train the BERT model |
| `--train-simple` | Train the lightweight model |
| `--evaluate` | Model evaluation, producing a report |
| `--demo` | Command-line interactive demo |
| `--all` | Full pipeline (prepare → train → evaluate) |
| `--list-config` | Print the current configuration |
| `--fix-env` | Environment repair |
| `--setup-model` | Download/configure the BERT model |
| `--pretrained-model <path>` | Override the pretrained model path |
| `--finetuned-model <path>` | Override the fine-tuned model path |
| `--device {cpu,cuda}` | Specify the compute device |
| `--batch-size <N>` | Batch size |
| `--num-epochs <N>` | Number of training epochs |
| `--max-length <N>` | Maximum sequence length |
| `--model-type {bert,qwen,simple}` | Choose the model type |

> To get **trustworthy metrics**, rerun `--train-bert` + `--evaluate` with a larger `--batch-size` and more `--num-epochs`, and confirm that the test set does not overlap the training set.

## 📁 Project structure

```text
电影评论情感分析系统/
│
├── 📂 app/                         # Web application
│   ├── 📂 static/                  # Static assets (CSS, JS, images)
│   ├── 📂 templates/index.html     # Main page
│   ├── 📂 configs/model_config.py  # Model configuration
│   └── app.py                      # Flask application entry point
│
├── 📂 cache/                       # huggingface / transformers cache
├── 📂 datasets/
│   ├── 📂 processed/               # train.csv / val.csv / test.csv / dataset_stats.json
│   ├── 📂 raw/                     # Raw movie review corpus
│   └── labels.json                 # Label definitions
│
├── 📂 models/
│   ├── 📂 bert-base-chinese/       # BERT pretrained model
│   ├── 📂 finetuned/               # Fine-tuned model
│   └── 📂 tokenizer/               # Tokenizer files
│
├── 📂 notebooks/                   # Data download and processing
├── 📂 results/                     # training_results.json / evaluation_results.json
├── 📂 scripts/
│   ├── 📂 src/                     # setup_bert_model / fix_environment / fix_issues / fix_pytorch
│   ├── data_preprocess.py          # Data preprocessing
│   ├── train_bert.py               # BERT training
│   └── evaluate_cpu.py             # Evaluation in a CPU environment
│
├── main.py                         # Main program entry point
└── req.txt                         # Dependency list
```

## 📌 Notes

- This project is a **course final assignment**, archived now that the course has ended; **no further maintenance is provided**.
- The evaluation numbers are contradictory, as clearly flagged in "Important note on the evaluation results" — please do not cite them as a performance claim.
- The corpus is public movie review data and contains no personal sensitive information.

<sub>Course final assignment · autumn 2025 · code and results kept for reference</sub>
