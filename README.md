# Automated Essay Scoring for English Language Learners

## 1. Project Overview

### Project Description

This project investigates whether linguistically motivated features can improve automated essay scoring (AES) for English language learners (ELLs).

Automated Essay Scoring systems are increasingly used in standardized testing and educational assessment. While traditional text-based models can capture lexical patterns in student writing, they may overlook linguistically meaningful characteristics related to language development.

This study compares a baseline model based on TF-IDF text features with an enhanced model that additionally incorporates lexical diversity and grammatical accuracy features.

### Research Question

**Does incorporating linguistically motivated features, including lexical diversity and grammatical accuracy, improve the performance of a supervised automated essay scoring system for predicting English language learners' writing proficiency levels compared with a TF-IDF baseline?**

### Research Objectives

The main objectives are to:

1. Develop a baseline automated essay scoring model using TF-IDF features.
2. Extract linguistically motivated features representing lexical diversity and grammatical accuracy.
3. Compare the baseline and enhanced models.
4. Evaluate whether these additional linguistic features improve the prediction of ordered writing proficiency levels.
5. Examine model errors to understand how well the models distinguish between different proficiency levels.

---

## 2. Dataset Description

### Dataset

The dataset used in this project is the **TOEFL11 Learner Corpus**, a collection of essays written by English language learners and released by Educational Testing Service (ETS).

The dataset contains **12,100 essays** with three ordered proficiency levels:

* Low
* Medium
* High

The corpus has been widely used in automated essay scoring and second-language learner language research.

### Dataset Purpose

The dataset is used to investigate the relationship between linguistic characteristics of learner writing and English writing proficiency.

The unit of analysis is an **individual essay**.

---

## 3. Data and File Overview

The primary dataset is stored as a CSV file.

| File                  | Description                                        | Format   |
| --------------------- | -------------------------------------------------- | -------- |
| `ETS_Corpus_full.csv` | Full TOEFL11 essay dataset used for the analysis   | CSV      |
| `README.md`           | Documentation for the research project and dataset | Markdown |
| `data_dictionary.csv` | Description of dataset variables                   | CSV      |

### Dataset Structure

The original dataset contains three primary variables:

| Variable   | Description                                         | Data Type             |
| ---------- | --------------------------------------------------- | --------------------- |
| `Filename` | Identifier corresponding to the original essay file | Categorical / String  |
| `label`    | English writing proficiency level                   | Categorical / Ordinal |
| `text`     | Full text of the learner's essay                    | Text                  |

---

## 4. Data Dictionary

### `Filename`

* **Description:** Identifier of the original essay file.
* **Type:** String
* **Role:** Essay identifier
* **Example:** `88.txt`
* **Missing values:** No missing values are expected.

### `label`

* **Description:** English writing proficiency level assigned to the essay.
* **Type:** Categorical / Ordinal
* **Possible values:**

  * `low`
  * `medium`
  * `high`
* **Interpretation:** The categories represent ordered levels of learner writing proficiency.
* **Model encoding:**

  * Low = 1
  * Medium = 2
  * High = 3

### `text`

* **Description:** Full written essay produced by an English language learner.
* **Type:** Unstructured text
* **Role:** Primary predictor information for the automated essay scoring models.
* **Processing:** The essay text was transformed into TF-IDF features for model training. Additional linguistic features were extracted from the text.

---

## 5. Derived Analysis Features

In addition to the original dataset variables, this project generated linguistically motivated features from the essay text.

### TF-IDF

TF-IDF was used to represent lexical information in the essays. These features formed the basis of the baseline model.

### Mean Segmental Type-Token Ratio (MSTTR)

MSTTR was used to measure lexical diversity while reducing the strong dependence of traditional Type-Token Ratio (TTR) on essay length.

Higher MSTTR values indicate greater lexical diversity.

### Subject-Verb Agreement (SVA) Error Rate

SVA error rate was used as a measure of grammatical accuracy.

The feature was extracted using rule-based grammatical error detection with spaCy.

Lower SVA error rates indicate greater grammatical accuracy.

The original project found that MSTTR increased across proficiency levels, while SVA error rates decreased as proficiency increased.

---

## 6. Methodological Information

### Data Processing

The analysis was conducted using Python in Google Colab.

The project used:

* Python 3.12.12
* pandas
* NumPy
* scikit-learn
* spaCy

pandas and NumPy were used for data processing, scikit-learn was used for TF-IDF vectorization, model training, tuning, and evaluation, and spaCy was used to extract grammatical features.

### Feature Construction

Two model specifications were developed:

**Model 1: TF-IDF Baseline**

The baseline model uses TF-IDF text representations as predictors.

**Model 2: Enhanced Model**

The enhanced model combines:

* TF-IDF features
* MSTTR
* SVA error rate

Numeric features were standardized before model fitting.

### Statistical / Machine Learning Model

A Ridge Regression model was used because the TF-IDF representation creates a high-dimensional and sparse feature space.

Ridge regression also provides regularization to reduce overfitting when combining sparse textual features with additional numerical linguistic features.

Because the proficiency labels are ordered, the model produced continuous predictions that could be evaluated using Quadratic Weighted Kappa (QWK).

### Hyperparameter Tuning

The Ridge regularization parameter α was tuned using three-fold cross-validation.

The candidate values were:

* 0.1
* 1.0
* 5.0
* 10.0

The optimal value for both models was α = 1.0 based on cross-validation QWK.

Predicted values were rounded and constrained to the valid proficiency range before being mapped back to the original proficiency categories.

---

## 7. Evaluation

The primary evaluation metric was **Quadratic Weighted Kappa (QWK)**.

QWK was selected because the target variable consists of ordered proficiency levels. Unlike simple accuracy, QWK takes the magnitude of disagreement into account and penalizes larger disagreements more heavily.

The enhanced model improved QWK from **0.595 to 0.600** compared with the TF-IDF baseline. Improvements in accuracy and weighted F1-score were relatively modest.

Error analysis showed that most classification errors occurred between adjacent proficiency levels, with relatively few direct errors between low- and high-proficiency essays.

---

## 8. Results Summary

The enhanced model, which incorporated lexical diversity and grammatical accuracy, performed better than the TF-IDF baseline across the evaluation metrics.

The largest improvement was observed in QWK.

These findings suggest that linguistically motivated features provide information beyond surface-level lexical similarity and may help distinguish essays with similar topical content but different levels of language development.

The results also suggest that the additional features are relatively interpretable and computationally lightweight, which may be useful in educational assessment settings.

---

## 9. Data Sharing and Access

The analysis dataset is based on the TOEFL11 Learner Corpus, which was released by Educational Testing Service (ETS).

The original corpus should be cited appropriately when used in research.

Because the dataset contains learner-written essays, users should review the original dataset's access conditions, terms of use, and any applicable restrictions before redistributing or publishing the raw data.

This project does not claim ownership of the original TOEFL11 corpus.

For reproducibility, the project documentation provides information about the dataset structure, variables, feature construction, software environment, and analytical workflow.

---

## 10. Reproducibility

The analysis was conducted in Google Colab using Python.

### Software

* Python 3.12.12
* Google Colab
* pandas
* NumPy
* scikit-learn
* spaCy

### Analysis Workflow

The general workflow is:

1. Load the TOEFL11 dataset.
2. Inspect the proficiency label distribution.
3. Preprocess essay text.
4. Generate TF-IDF representations.
5. Extract MSTTR and SVA error rate.
6. Construct the baseline TF-IDF model.
7. Construct the enhanced model using TF-IDF + MSTTR + SVA error rate.
8. Tune Ridge regression using three-fold cross-validation.
9. Evaluate models using QWK, accuracy, weighted F1-score, and confusion matrices.
10. Conduct error analysis.
11. Compare the baseline and enhanced models.

---

## 11. Metadata Standard

### Selected Standard: Data Documentation Initiative (DDI)

This project uses the **Data Documentation Initiative (DDI)** as the main reference for metadata documentation.

DDI was selected because it provides a structured framework for documenting research data in the social sciences. The standard is relevant to this project because the dataset represents human learning and language behavior and includes information about the dataset, variables, methodology, and access.

Using a metadata standard also improves the clarity, consistency, and potential reproducibility of the research project.

---

## 12. Researcher Information

**Researcher:** Xujun Wang
**Institution:** Teachers College, Columbia University
**Program:** M.S. Applied Statistics

### ORCID

ORCID: *To be added*

---

## 13. DOI

DOI: *To be added if a DOI has been assigned to this project.*

---

## 14. Citation

If this project is used or referenced, please cite the original TOEFL11 Learner Corpus and this research project appropriately.

### Key Dataset Reference

Blanchard, D., Tetreault, J., Higgins, D., Cahill, A., & Chodorow, M. (2013). TOEFL11: A corpus of non-native English.

---

## 15. Limitations

This project has several limitations.

First, the analysis focuses on a limited set of linguistically motivated features, primarily lexical diversity and subject-verb agreement accuracy.

Second, the dataset contains an imbalanced distribution of proficiency labels, with approximately 1,330 low-, 6,568 medium-, and 4,202 high-proficiency essays.

Third, the project uses a feature-based Ridge Regression approach rather than more recent neural language models. Future work could incorporate additional syntactic, discourse-level, or neural representation features.

Finally, automated essay scoring should be interpreted as an assessment support tool rather than a complete replacement for human judgment.

---

## 16. References

Attali, Y., & Burstein, J. (2006). Automated essay scoring with e-rater® v.2. *Journal of Technology, Learning, and Assessment, 4*(3), 1–30.

Crossley, S. A., Weston, J. L., McLain Sullivan, S. T., & McNamara, D. S. (2011). The development of writing proficiency as a function of linguistic features. *Journal of Second Language Writing, 20*(2), 119–133.

Kyle, K., & Crossley, S. A. (2015). Automatically assessing lexical sophistication: Indices, tools, findings, and application. *TESOL Quarterly, 49*(4), 757–786.

McCarthy, P. M., & Jarvis, S. (2010). MTLD, vocd-D, and HD-D: A validation study of sophisticated approaches to lexical diversity assessment. *Behavior Research Methods, 42*(2), 381–392.

Williamson, D. M., Xi, X., & Breyer, F. J. (2012). A framework for evaluation and use of automated scoring. *Educational Measurement: Issues and Practice, 31*(1), 2–13.
