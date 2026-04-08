# Mobile Plan Recommendation Engine

![Demo](https://viet-in-tech.github.io/mobile-plan-demo.gif)
### Megaline Telecom — Plan Classification Model

**Portfolio write-up:** [Recommending the Right Plan: Machine Learning for Mobile Carriers](https://viet-in-tech.github.io/megaline-plan-recommendation.html)

**TripleTen Data Science Program · Sprint 8 — Introduction to Machine Learning**

**Completed:** December 15, 2025

---

## Project Overview

Megaline, a mobile carrier, wants to move customers off legacy plans and onto one of two modern plans: **Smart** or **Ultra**. The goal is to build a classification model that recommends the right plan based on a customer's usage behavior.

**Project requirement:** Achieve a minimum **accuracy ≥ 75%** on the held-out test set.

---

## Dataset

- **Source:** `/datasets/users_behavior.csv`
- **Size:** 3,214 customers × 5 columns
- **Target column:** `is_ultra` (0 = Smart, 1 = Ultra)
- **Class split:** ~69.4% Smart / 30.6% Ultra
- **Missing values:** None — all 3,214 rows are complete

**Features:**

| Feature | Description |
|---------|-------------|
| calls | Number of calls made per month |
| minutes | Total call duration (minutes) |
| messages | Number of text messages |
| mb_used | Internet traffic used (MB) |

---

## Workflow

### Phase 1 — Import Libraries & Load Data
- pandas, scikit-learn classifiers and metrics

### Phase 2 — Exploratory Data Analysis
- Dataset shape (3,214 × 5), data types, statistical summary
- No missing values found
- Binary classification problem (Smart vs Ultra)

### Phase 3 — Feature Scaling Consideration
- `mb_used` ranges ~0–49,745 vs `calls` ~0–244 (very different scales)
- **Decision:** Tree-based methods (Decision Tree, Random Forest) use binary splits on individual features — scale differences do not affect performance
- Feature scaling skipped; would be required for distance-based algorithms

### Phase 4 — Model Training & Validation
- Stratified 60/20/20 train/validation/test split
- Two validation attempts: baseline → hyperparameter tuning

**Validation Results:**

| Model | Dataset | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|---|
| Decision Tree (Baseline) | Validation | 76.5% | 67.8% | 40.6% | 50.8% |
| Random Forest (100 trees) | Validation | 79.5% | 71.4% | 52.1% | 60.2% |
| Decision Tree (Tuned) | Validation | 76.5% | 64.5% | 47.4% | 54.7% |
| Random Forest (10,000 trees) | Validation | 79.9% | 73.7% | 51.0% | 60.3% |

### Phase 5 — Final Test Evaluation

Selected model: **Random Forest (10,000 trees, max_depth=10)**

---

## Final Test Results

| Metric | Score |
|--------|-------|
| Accuracy | 81.8% |
| Precision | 77.5% |
| Recall | 53.2% |
| F1 Score | 63.1% |

**Target Accuracy ≥ 75% → PASSED** (margin: +6.8%)

---

## Why Accuracy — Not F1

For this project, **accuracy and precision** are the most relevant metrics:

- **Accuracy:** "How often does the model recommend the right plan across all customers?"
- **Precision:** "When the model recommends Ultra, how often is that correct?"

Missing an Ultra recommendation is not catastrophic — a customer can upgrade later. This makes recall less critical, and therefore F1 less relevant, for this specific business case.

---

## Technologies

- Python 3.8
- pandas
- scikit-learn (DecisionTreeClassifier, RandomForestClassifier, train_test_split, accuracy_score, precision_score, recall_score, f1_score, confusion_matrix)

---

## Project Status

✅ Approved — TripleTen Data Science Program, Sprint 8
