# Rule-Based Classification for Donor Age Prediction


I implement and compare **two core approaches** to utilize and evaluate these rules on a biomedical dataset.

---

## Loading and Cleaning Rules

Before applying the rules, we:

- Load them from a plain text file.
- Remove duplicates.
- Remove rules that are **strict subsets** of other rules (i.e., redundant conditions).

---

## Evaluating Rule-Based Classifier by Sorting Strategy

In this **heuristic approach**, we:

1. Evaluate each rule **individually** for its precision, recall, and F1-score.
2. Sort all rules based on one of those metrics.
3. Build a classifier by taking the top-N rules and predicting `donor_is_old = True` if **any of the rules match**.
4. Plot the overall classifier performance (precision, recall, F1) vs. number of rules used.

This is **not machine learning** in the classical sense — it is a scoring-based, interpretable rule selection technique.

---

## Logistic Regression Using Rule-Based Features

In this **machine learning approach**, we:

1. Treat each rule as a binary feature (0/1) — whether it matches a given data row.
2. Build a feature matrix `X` with shape `(samples × rules)`.
3. Train a logistic regression model with **L1 regularization**, which automatically selects the most important rules.
4. Evaluate model performance on a test set and visualize which rules had the highest influence (non-zero coefficients).

This allows the model to **learn combinations and weights of rules** rather than rely solely on individual metric ranking.

---

## Missing Data Handling

For both approaches, we carefully handle missing values (`NA`) in the dataset:

- If **any required biomarker is missing** in a row for a given rule, that row is **excluded from evaluation** for that rule.
- This ensures we don't make false predictions or distort performance metrics due to incomplete data.

We chose this strict approach to maintain result reliability and avoid misclassification driven by uncertainty.

---

## Results

| Method               | Top-N Rules | Precision | Recall | F1-score |
|----------------------|-------------|-----------|--------|----------|
| Top-N Heuristic      | 5           | 0.8333    | 1.0000 | 0.9091   |
| Logistic Regression  | 2           | 0.8571    | 1.0000 | 0.9231   |

Both methods achieve perfect recall, but logistic regression slightly outperforms the heuristic approach in terms of precision and F1-score, using fewer rules.

**Heuristic (Top 5):**
- `GDF_15`
- `NOT HCT AND NOT Lymphocytes AND NOT Notch_1`
- `SOST`
- `Neutrophils`
- `NOT Hb AND NOT Lymphocytes AND NOT Notch_1`

**Logistic Regression (Top 2):**
- `GDF_15`
- `NOT Hb AND NOT Lymphocytes AND NOT Notch_1`


---

## Files

- `rules.txt` – original rules in plain text.
- `dataset.tsv` – biomedical dataset used for evaluation.
- `rule-based_classification.ipynb` – full analysis pipeline with plots, metrics and model results.

---

## How to Run

Open the notebook in JupyterLab or VS Code and execute all cells from top to bottom.  
You can easily switch between methods, tweak thresholds, or add new rules for experimentation.
