# Group Contributions


> Each member contributed approximately equally to the overall project.
> The breakdown below reflects primary ownership of each component.
> All members reviewed, tested, and gave feedback on every section before final submission.

---

## Member contributions

### Faly — Team Lead / Setup / Data Loading / Experiments 1–2

**Notebook sections:** Section 1 (Setup & Installs), Section 2 (Load Data), Experiments 1 and 2

| Task | Description |
|---|---|
| Project coordination | Initialised the GitHub repository, set up branch structure, managed pull request reviews, and enforced the section-by-section push workflow.

### Kelvin — EDA / Experiments 3–4

**Notebook sections:** Section 3 (Exploratory Data Analysis), Experiments 3 and 4

| Task | Description |
|---|---|
| Label frequency visualisation | Produced the label frequency bar chart (Section 3.1), revealing the 33× head/tail imbalance between label 3.b.2 (1,040 positives) and 3.6.1 (31 positives). Connected this finding to the decision to use `class_weight='balanced'` in later experiments. |
| Label cardinality analysis | Computed and plotted the labels-per-sample distribution (Section 3.2), confirming average cardinality of 1.97 and a maximum of 10 labels per document. |
| Document length distribution | Produced the word-count histogram (Section 3.2), identifying the long tail up to 2,838 words and the median of 213 words. This informed the `max_features` and `min_df` choices in TF-IDF. |
| Label co-occurrence heatmap | Built the normalised label co-occurrence matrix (Section 3.3) showing that many SDG-3 sibling indicators (3.1.1/3.1.2, 3.8.1/3.8.2) share vocabulary, which later explained why Complement Naive Bayes collapsed. |
| Baseline Hamming Loss | Computed the all-zeros baseline (HL = 0.073) as a sanity floor for all experiments. |
| Experiment 3 — LinearSVC | Implemented OvR LinearSVC (C=0.5) on TF-IDF features to compare a maximum-margin linear model against LR on the same representation. Documented performance vs Exp 1. |
| Experiment 4 — SBERT + LR | Implemented SBERT encoding (all-MiniLM-L6-v2, 384-d) and ran OvR Logistic Regression on dense embeddings alone. Documented how semantic embeddings compare to sparse TF-IDF and identified which label groups benefit most from dense representations. |

---

### Richard — Preprocessing / Experiments 5–6

**Notebook sections:** Section 4 (Preprocessing Pipeline), Experiments 5 and 6



### Henriette — Feature Engineering / Experiments 7–8 / Inference

**Notebook sections:** Section 5 (Feature Engineering), Experiments 7 and 8, Section 8 (Inference)


## Shared contributions (all members)

| Task | All members |
|---|---|
| Report writing | Each member wrote the section of the report corresponding to their notebook section. All members reviewed and edited the full draft before submission. |
| Demo video | All four members appear and present in the 7–10 minute demo video. Each member presents their own section. |
| Code review | Each member reviewed at least one other member's pull request before it was merged to main. |
| Experiment analysis | All members participated in the discussion of results and the writing of the Discussion section. |
| README | Faly drafted; all members reviewed. |

---

## Contribution summary (estimated percentage)

| Member | Estimated contribution |
|---|---|
| Faly | ~25 % |
| Kelvin | ~25 % |
| Richard | ~25 % |
| Henriette | ~25 % |

---

