---
layout: post
author: THIN THIN HLAING
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background

This team project focuses on enhancing sportswear mobile application user experience through data-driven review analytics. Major sportswear brands such as Nike, Adidas, Puma, and Gymshark rely heavily on user reviews to monitor satisfaction, identify operational issues, and improve product quality.

However, manually analysing thousands of user reviews is inefficient and difficult to scale. Therefore, the project aims to leverage data science techniques to systematically extract sentiment patterns, engagement behaviour, and recurring complaint themes from Google Play Store reviews.

The overall team objective is to transform unstructured textual feedback into actionable business insights.

My assigned responsibility was **Objective 2 – Sentiment Quantification and Pattern Analysis**.

**Business Objective:**  
To quantify user sentiment across brands and develop a predictive text classification model capable of automatically identifying dissatisfied customers based on review text. The goal is to support scalable monitoring and early detection of recurring service or operational issues.

---
 
## Work Accomplished

### Data Preparation

Additional validation included score range checks and brand value verification to ensure dataset integrity.

- Loaded and validated 6,446 Google Play reviews across Nike, Adidas, Puma, and Gymshark.
- Performed structural cleaning including column standardisation, datetime conversion, score validation, and brand value verification.
- Dropped `reply_content` due to 93% missing values.
- Engineered features including:
  - `year_month` (for temporal trend analysis)
  - Review length metrics (character and word count)
  - Rating-derived sentiment labels

Preprocessing applied to create `clean_content`:
- Lowercasing and whitespace normalisation  
- Normalisation of negations (e.g., "can't" → "cant")  
- Removal of punctuation, numbers, emojis, and non-ASCII characters  
- Stopword removal (while preserving negation words such as "not", "never")  
- Tokenisation using NLTK  
- No stemming (to preserve word clarity)  

Note: Removal of non-ASCII characters may reduce representation of multilingual content.

A total of 118 rows were removed after preprocessing due to empty processed text.

---

### Modelling

Initial modelling adopted a 3-class sentiment classification approach:

- BoW + Naive Bayes  
- BoW + Logistic Regression  
- TF-IDF + Naive Bayes  
- TF-IDF + Logistic Regression  
- TF-IDF + SVM  

Due to strong class imbalance (Neutral ≈ 4%), minority-class recall performance was limited.

A binary redesign (Negative vs Positive) was implemented to improve dissatisfaction detection reliability. This redesign simplified the classification task and improved minority-class performance under severe class imbalance conditions.

Final selected model:  
**TF-IDF (Unigram) + Logistic Regression (class_weight="balanced")**

---

### Evaluation

Because of class imbalance, overall accuracy was not treated as the primary evaluation metric.

Instead, evaluation focused on:

- Macro F1-score  
- Balanced accuracy  
- Confusion matrix analysis  
- Stratified train-test split  

The final binary model demonstrated stronger Negative class detection and improved balanced performance compared to 3-class models.

---

## Recommendation and Analysis

### Key insights:

- 74% of reviews are positive (4–5 stars), indicating strong overall satisfaction.
- Negative reviews receive disproportionately higher thumbs-up engagement, suggesting complaints resonate with other users.
- Complaint themes focus on refund delays, login issues, checkout failures, and operational reliability.
- Approximately 40% of reviews contain three words or fewer, increasing modelling uncertainty and limiting contextual depth.

### Business Recommendations

Following the binary redesign, recommendations focus specifically on reliable dissatisfaction detection (Negative vs Positive) to support early intervention.

1. Prioritise high-engagement negative reviews as early warning indicators.
2. Strengthen customer support workflows and operational reliability.
3. Implement continuous sentiment monitoring dashboards.
4. Track emerging complaint themes on a monthly basis.
5. Integrate recurring complaint themes into continuous product improvement cycles.

---

## AI Ethics

**Privacy:**  
The dataset consists of publicly available Google Play reviews. No personal identifiable information was intentionally collected.

**Fairness:**  
Class imbalance was addressed using macro F1-score and class-weighted models to reduce majority-class bias.

**Transparency:**  
Sentiment labels were explicitly derived from rating scores using rule-based mapping, ensuring explainability of ground truth construction.

**Accountability:**  
Model outputs support decision-making and monitoring but should not replace human judgement.

**Reliability:**  
Periodic retraining is recommended as app versions evolve and user behaviour changes.

This project demonstrates the practical application of supervised text classification for business-oriented dissatisfaction detection under real-world class imbalance conditions.

---
  
## Source Codes and Datasets
Project Repository:
 - https://github.com/THINTHINHLAING/ITD214-Sportswear-App-Review-Analytics
