# SDG 3 Indicator Multi-Label Text Classification

**Group Assignment 2** · Machine Learning for NLP



> Automatically classifying international development text documents against 12 SDG 3 health indicators using TF-IDF, Sentence-BERT, and hybrid feature representations.

---

## Results Summary

| Experiment | Configuration | Hamming Loss ↓ | F1-Macro | F1-Weighted |
|---|---|---|---|---|
| **Exp 8 ★** | **Hybrid (TF-IDF+SBERT) + per-label threshold** | **0.0649** | **0.2739** | **0.7772** |
| Exp 6 | TF-IDF + LR + per-label threshold | 0.0661 | 0.2308 | 0.7783 |
| Exp 7 | SBERT + LR + per-label threshold | 0.0684 | 0.2683 | 0.7870 |
| Exp 2 | TF-IDF + Random Forest | 0.0698 | 0.2335 | 0.7668 |
| Exp 1 | TF-IDF + Logistic Regression (Baseline) | 0.0728 | 0.1737 | 0.7004 |
| Exp 4 | SBERT + Logistic Regression | 0.0735 | 0.1772 | 0.7093 |
| Exp 3 | TF-IDF + LinearSVC | 0.0755 | 0.1664 | 0.6758 |
| Exp 5 | TF-IDF + LR (class_weight=balanced) | 0.0786 | 0.2523 | 0.7389 |

All-zeros baseline: **HL = 0.1976** · Best model improvement: **67%**

---

## Repository Structure

```
├── text_classification.ipynb   # Main notebook — complete pipeline
├── README.md                   # This file
├── requirements.txt            # Python dependencies
└── outputs/
    ├── predictions.csv         # Test set predictions (generated on run)
  
```

---

## Quick Start (Google Colab)

### Step 1 — Open the notebook

Click the **Open in Colab** badge above, or upload `text_classification.ipynb` directly to [colab.research.google.com](https://colab.research.google.com).

### Step 2 — Enable GPU (recommended)

Go to **Runtime → Change runtime type → T4 GPU**. This speeds up Sentence-BERT encoding from ~15 minutes to ~2 minutes.

### Step 3 — Upload the datasets

In the Colab **Files panel** (folder icon on the left), upload:
- `Devex_train.csv`
- `Devex_test_questions.csv`

Both files should appear at the root of the Colab session (`/content/`).

### Step 4 — Run all cells

Go to **Runtime → Run all**. The notebook runs end-to-end without manual intervention.

**Expected total runtime:** ~25–35 minutes on CPU, ~10–15 minutes on T4 GPU.

### Step 5 — Download predictions

After completion, `predictions.csv` will appear in the Files panel. Right-click → Download.

---

## Notebook Sections

| Section | Description |
|---|---|
| 1. Setup & Installs | Installs `scikit-multilearn`, `sentence-transformers`, `imbalanced-learn` |
| 2. Load Data | CSV loading with multi-encoding fallback; auto-detects text and label columns |
| 3. EDA | Label frequency, co-occurrence heatmap, text length distribution, baseline HL |
| 4. Preprocessing | HTML stripping, lemmatisation, domain-aware stopword removal |
| 5. Feature Engineering | TF-IDF (30k features, bigrams) and Sentence-BERT (384-dim) encoding |
| 6. Experiments | 8 experiments, each documented with rationale and outcome |
| 7. Results | Comparison table, bar chart, per-label F1, learning curve |
| 8. Inference | Retrain on full data, threshold tuning, generate `predictions.csv` |

---

## Dependencies

```
pandas
numpy
matplotlib
seaborn
scikit-learn
scikit-multilearn
sentence-transformers
imbalanced-learn
nltk
scipy
```

All are installed automatically in Section 1 of the notebook. For local use:

```bash
pip install -r requirements.txt
```

---

## Dataset

| Split | Samples | Columns |
|---|---|---|
| Train (`Devex_train.csv`) | 2,995 | Unique ID, Type, Text, Label 1–12 |
| Test (`Devex_test_questions.csv`) | 998 | Unique ID, Type, Text |

Labels represent SDG 3 sub-indicators (e.g., `3.c.1 - Health worker density and distribution`). Missing values in label columns indicate the label is not applicable (encoded as 0). Label 1 appears in 100% of training samples; Labels 11–12 have zero positive examples.

---

## Methodology Overview

### Problem Type
Multi-label binary classification (12 labels, one per SDG 3 indicator). Each sample can belong to 0–12 labels simultaneously. Evaluated using **Hamming Loss** (lower = better).

### Pipeline
1. **Preprocessing** — lowercasing, HTML/URL removal, tokenisation, domain-aware stopword filtering, WordNet lemmatisation
2. **Features** — TF-IDF (unigrams + bigrams, 30k vocabulary) and/or Sentence-BERT (`all-MiniLM-L6-v2`, 384-dim)
3. **Classifier** — `OneVsRestClassifier` wrapping calibrated Logistic Regression, Random Forest, or LinearSVC
4. **Threshold tuning** — per-label grid search over [0.10, 0.90] on the validation set to minimise label-specific Hamming Loss
5. **Inference** — retrain on full training data, apply tuned thresholds, output `predictions.csv`

### Key Finding
Per-label threshold tuning (mean optimal threshold = 0.30 vs default 0.50) was the single most impactful technique, reducing HL by ~9% over the same model without tuning. Combining TF-IDF and SBERT features in a hybrid representation provided further gains.

---

## Reproducing Specific Experiments

All experiments run sequentially in the notebook. To re-run only a specific experiment, ensure Sections 1–5 have been run first (they set up the feature matrices), then run the relevant cell in Section 6.

To change the final inference model, edit the `BEST_FEATURES` variable in Section 8:
```python
BEST_FEATURES = 'hybrid'   # Options: 'tfidf', 'sbert', 'hybrid'
```

---

## Output Files

After a full run, the following files are saved to the Colab working directory:

| File | Description |
|---|---|
| `predictions.csv` | Test set predictions — 998 rows × 15 columns |
| `eda_label_frequency.png` | Bar chart of label counts |
| `eda_distributions.png` | Labels-per-sample and text-length histograms |
| `eda_cooccurrence.png` | Normalised label co-occurrence heatmap |
| `results_hamming_comparison.png` | Hamming Loss bar chart across experiments |
| `results_per_label_f1.png` | Per-label F1 for the best model |
| `results_learning_curve.png` | Learning curve (train vs val F1) |

---

## Group Members and Contributions
https://docs.google.com/spreadsheets/d/1bi5tkThMaKFuUDYXujSRA53r9ciqcK374-ZxerDWpEs/edit?usp=sharing

 |

📹 **Demo Video:** [https://drive.google.com/file/d/1x1NbyRtOACj6WReW4EXn4aEPpJUIVxt5/view?usp=sharing]  
📄 **Report:** `Group9_Assignment2.pdf`

---

## Citation Style

This project follows **IEEE** citation style in the accompanying academic report.