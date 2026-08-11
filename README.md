# Malware Detection Using Data Mining Techniques

<p align="center">

<strong>A Comparative Study of Classification Algorithms for
Multi-Class Malware Detection`</strong>
</p>

<p align="center">

<img src="https://img.shields.io/badge/Domain-Cybersecurity-red?style=for-the-badge" alt="Cybersecurity">
<img src="https://img.shields.io/badge/Discipline-Data%20Mining-blue?style=for-the-badge" alt="Data Mining">
<img src="https://img.shields.io/badge/Models-3-orange?style=for-the-badge" alt="Models">
<img src="https://img.shields.io/badge/Dataset-Microsoft%20BIG%202015-green?style=for-the-badge" alt="Dataset">
</p>

<p align="center">

<em>End-to-end data mining pipeline for classifying malware
families using feature selection, class balancing, and supervised
machine learning.</em>
</p>


------------------------------------------------------------------------

## 📌 Project Overview

Malware continues to evolve rapidly, making traditional signature-based
detection increasingly difficult to rely on for emerging and polymorphic
threats. This project investigates a **data mining and machine
learning-based approach to multi-class malware classification**.

The study compares three supervised classification algorithms:

- **Decision Tree**
- **Random Forest**
- **Gaussian Naive Bayes**

The experiments use a pre-processed version of the **Microsoft Malware
Classification Challenge (BIG 2015)** dataset containing numeric
features derived from PE executables.

The proposed pipeline combines:

1.  Data preprocessing
2.  Missing-value handling
3.  Variance-based feature reduction
4.  Recursive Feature Elimination (RFE)
5.  Min-Max normalization
6.  Stratified train/test splitting
7.  SMOTE-based class balancing
8.  Supervised model training
9.  Accuracy and ROC-AUC evaluation
10. Confusion-matrix analysis
11. Random Forest feature-importance analysis

The strongest model was **Random Forest**, achieving approximately
**99.05% accuracy**, a **0.98 macro F1-score**, and a **0.9996 ROC-AUC**
according to the project results.

------------------------------------------------------------------------

## 🎯 Research Objectives

The project was designed around three primary objectives:

- Build an end-to-end data mining pipeline for malware-family
  classification.
- Compare the performance of Decision Tree, Random Forest, and Naive
  Bayes classifiers.
- Identify which selected numerical features contribute most to malware
  classification.

### Scope

The study focuses on a **preprocessed static feature set derived from PE
executables** and does not perform dynamic sandbox analysis. The
reported results are specific to the Microsoft BIG 2015 dataset used in
the project.

------------------------------------------------------------------------

## 🧠 Methodology

The overall workflow can be represented as:

``` text
                 ┌──────────────────────┐
                 │   Malware Dataset    │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Data Preprocessing   │
                 │ • Numeric Features   │
                 │ • Missing Values     │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Feature Reduction    │
                 │ • Variance Threshold │
                 │ • RFE                 │
                 │ • Top 30 Features    │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Normalization        │
                 │    Min-Max Scaling   │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Train / Test Split   │
                 │   70% / 30%          │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │      SMOTE           │
                 │ Class Balancing      │
                 └──────────┬───────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
      ┌────────────┐ ┌────────────┐ ┌────────────┐
      │ Decision   │ │  Random    │ │   Naive    │
      │   Tree     │ │  Forest    │ │   Bayes    │
      └─────┬──────┘ └─────┬──────┘ └─────┬──────┘
            │              │              │
            └──────────────┼──────────────┘
                           ▼
                 ┌──────────────────────┐
                 │ Model Evaluation     │
                 │ • Accuracy           │
                 │ • ROC-AUC            │
                 │ • F1-score           │
                 │ • Confusion Matrix   │
                 └──────────────────────┘
```

------------------------------------------------------------------------

## 📊 Models Evaluated

### 1. Decision Tree

A Decision Tree was used as an interpretable baseline classifier. The
implemented configuration includes:

- `criterion = gini`
- `max_depth = 15`
- `min_samples_split = 5`
- `min_samples_leaf = 2`
- `random_state = 42`

### 2. Random Forest

Random Forest was evaluated as the primary ensemble classifier. The
configuration includes:

- `n_estimators = 200`
- `max_features = sqrt`
- `max_depth = None`
- `random_state = 42`
- Parallel processing enabled with `n_jobs = -1`

Random Forest also provided feature-importance estimates using mean
decrease in impurity.

### 3. Gaussian Naive Bayes

Gaussian Naive Bayes was included as a probabilistic baseline:

- `var_smoothing = 1e-9`

Its comparatively lower performance provides a useful contrast against
tree-based models.

------------------------------------------------------------------------

## 📈 Experimental Results

Model Accuracy ROC-AUC Performance

------------------------------------------------------------------------

**Random Forest** **99.05%** **0.9996** 🏆 Best **Decision Tree**
**97.39%** **0.9667** Strong **Naive Bayes** **64.06%** **0.9311**
Baseline

### Key Findings

**Random Forest** substantially outperformed the other evaluated
classifiers.

The results indicate that:

- Ensemble tree methods are highly effective for the selected malware
  feature representation.
- Feature selection can reduce the dimensionality of the input while
  retaining strong predictive performance.
- SMOTE helps address severe class imbalance in the training data.
- Naive Bayes performed considerably worse, suggesting that its
  assumptions do not adequately capture the statistical relationships
  present in the selected features.
- A relatively small subset of numerical features contributed strongly
  to the Random Forest decisions.

------------------------------------------------------------------------

## ⚖️ Handling Class Imbalance

The dataset exhibited substantial differences in class frequencies.

To address this issue, **SMOTE (Synthetic Minority Over-sampling
Technique)** was applied **only to the training data** after the
train/test split.

This is important because balancing the test set would distort the
evaluation of how the model performs on the original test distribution.

The project pipeline therefore follows:

``` text
Original Dataset
      │
      ▼
Train / Test Split
      │
      ├──────────────► Test Set
      │                 (unchanged)
      │
      ▼
Training Set
      │
      ▼
    SMOTE
      │
      ▼
Balanced Training Data
      │
      ▼
Model Training
```

------------------------------------------------------------------------

## 🔬 Feature Engineering

The project applies multiple stages of feature reduction and
transformation.

### Step 1 --- Numerical Feature Selection

Non-numeric columns are removed so that the classification pipeline
operates on numerical features.

### Step 2 --- Missing-Value Imputation

Missing numerical values are replaced using the corresponding feature
median.

### Step 3 --- Variance Threshold

Features with variance below `0.001` are removed.

### Step 4 --- Recursive Feature Elimination

Recursive Feature Elimination (RFE) with a Random Forest estimator
selects up to the **top 30 features**.

### Step 5 --- Min-Max Scaling

The selected features are normalized using `MinMaxScaler`.

This produces a compact feature representation before model training.

------------------------------------------------------------------------

## 🗂️ Dataset

The project uses a **preprocessed version of the Microsoft Malware
Classification Challenge (BIG 2015)** dataset.

**Original source:**

> Kaggle --- Microsoft Malware Classification Challenge (BIG 2015)

<https://www.kaggle.com/c/malwareclassification>

The paper describes the working dataset as containing **68 numeric
features**, from which the pipeline selects the top 30 features through
feature selection.

> **Note:** The dataset itself is not included in this repository unless
> explicitly added separately, due to dataset size and
> licensing/distribution considerations.

------------------------------------------------------------------------

## 🛠️ Technology Stack

Technology Purpose

------------------------------------------------------------------------

**Python** Core implementation **Pandas** Data loading and manipulation
**NumPy** Numerical computation **Scikit-learn** Feature selection,
preprocessing, models, metrics **Imbalanced-learn** SMOTE class
balancing **Matplotlib** Visualization **Seaborn** Confusion-matrix
visualization **Kaggle** Dataset / notebook environment

------------------------------------------------------------------------

## 📁 Suggested Repository Structure

A clean GitHub repository can be organized as follows:

``` text
malware-detection-data-mining/
│
├── README.md
├── Malware.md
│
├── notebook/
│   └── malware_detection.ipynb
│
├── src/
│   └── malware_detection.py
│
├── results/
│   ├── rf_confusion_matrix.png
│   └── rf_feature_importance.png
│
├── report/
│   └── paper.pdf
│
└── requirements.txt
```

If the repository contains only the paper and notebook, unnecessary
folders can be omitted.

------------------------------------------------------------------------

## ▶️ Reproducibility

The experimental pipeline follows this sequence:

``` python
# 1. Load dataset
# 2. Separate features and target
# 3. Keep numerical features
# 4. Fill missing values with median
# 5. Apply variance threshold
# 6. Select top 30 features using RFE
# 7. Apply Min-Max normalization
# 8. Perform stratified 70/30 train-test split
# 9. Apply SMOTE to training data
# 10. Train Decision Tree, Random Forest and Gaussian Naive Bayes
# 11. Evaluate accuracy and ROC-AUC
# 12. Generate Random Forest confusion matrix
# 13. Analyze Random Forest feature importance
```

The paper's reproducible code uses a fixed `random_state=42` for the
major randomized operations, helping make the experiment more consistent
across runs.

------------------------------------------------------------------------

## 📌 Visual Analysis

The study includes two important Random Forest visualizations:

### Feature Importance

The feature-importance analysis identifies the numerical feature indices
that contributed most strongly to the Random Forest's predictions.

### Confusion Matrix

The Random Forest confusion matrix demonstrates the model's
classification performance across the malware classes and provides a
class-level view of prediction errors.

------------------------------------------------------------------------

## ⚠️ Limitations

The study has several important limitations:

1.  **Static analysis only** --- the project does not perform dynamic
    behavioral or sandbox-based malware analysis.
2.  **Dataset dependency** --- results are specific to the Microsoft BIG
    2015 dataset and its preprocessing.
3.  **Feature representation** --- the experiments use a preprocessed
    numerical feature set rather than raw executable binaries.
4.  **Model scope** --- only three classical machine learning algorithms
    are compared.
5.  **Generalization** --- the reported performance should not be
    interpreted as proof that the same accuracy will be achieved on
    modern, unseen malware families or real-world operational
    environments.

These limitations are important when interpreting the near-perfect
Random Forest performance.

------------------------------------------------------------------------

## 🚀 Future Work

Potential extensions of this work include:

- Evaluating the pipeline on newer malware datasets such as EMBER.
- Comparing against gradient-boosting methods such as LightGBM or
  XGBoost.
- Investigating deep learning approaches.
- Combining static and dynamic malware features.
- Performing cross-dataset validation.
- Evaluating robustness against adversarially modified malware samples.
- Investigating explainable AI techniques for malware classification.
- Testing the pipeline on previously unseen malware families.

------------------------------------------------------------------------

## 📚 References

The project references foundational and supporting work including:

1.  Idika and Mathur (2007) --- malware detection method categorization.
2.  Kolter and Maloof (2006) --- n-gram byte-sequence analysis for
    malware classification.
3.  Ye et al. (2017) --- survey of machine learning approaches to
    malware detection.
4.  Anderson and Roth (2018) --- EMBER dataset and machine
    learning-based malware detection.
5.  Breiman, L. --- **Random Forests**, *Machine Learning*, vol. 45, no.
    1, pp. 5--32, 2001.
6.  Kaggle --- **Microsoft Malware Classification Challenge (BIG
    2015)**.
7.  Scikit-learn Developers --- **scikit-learn: Machine Learning in
    Python**.
8.  AV-TEST Institute --- **AV-TEST Security Report 2022/2023**.

------------------------------------------------------------------------

## 👥 Authors

### Rafid Shabab Remal Patwary

Department of Computer Science and Engineering  
United International University, Dhaka, Bangladesh

### Kazi Golam Sazid Hasan

Department of Computer Science and Engineering  
United International University, Dhaka, Bangladesh

### Yousuf Nobin

Department of Computer Science and Engineering  
United International University, Dhaka, Bangladesh

### Supervisor

**Dr. Ohidujjaman**  
Associate Professor  
Department of Computer Science and Engineering  
United International University, Dhaka, Bangladesh

------------------------------------------------------------------------

## 🎓 Academic Context

This project was developed as part of a **Data Mining course project**
and focuses on applying classical data mining and machine learning
techniques to a real-world cybersecurity classification problem.

The project demonstrates the complete workflow from **raw/preprocessed
data → feature engineering → class balancing → model training →
evaluation → interpretation**.

------------------------------------------------------------------------

## 📄 Research Paper

The full research paper is available in this repository:

**[Malware Detection Using Data Mining Techniques: A Comparative Study
of Classification Algorithms](./Malware.md)**

------------------------------------------------------------------------

## ⭐ Conclusion

This study demonstrates that carefully designed data mining pipelines
can achieve strong performance in multi-class malware classification.

Among the evaluated models, **Random Forest provided the strongest
overall performance**, reaching approximately **99.05% accuracy** and
**0.9996 ROC-AUC** on the reported evaluation.

The results emphasize the effectiveness of combining:

> **Feature Selection + Normalization + SMOTE + Ensemble Learning**

for malware classification using a compact numerical feature
representation.

------------------------------------------------------------------------

<p align="center">

<strong>Malware Detection • Data Mining • Machine Learning •
Cybersecurity</strong>
</p>

