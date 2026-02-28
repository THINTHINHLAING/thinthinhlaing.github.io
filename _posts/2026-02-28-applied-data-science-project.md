---
layout: post
author: THIN THIN HLAING
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background

This team project focuses on enhancing sportswear mobile application user experience through data-driven review analytics. Major sportswear brands such as Nike, Adidas, Puma, and Gymshark rely heavily on user reviews to monitor satisfaction, identify operational issues, and improve product quality.

However, manually analysing thousands of user reviews is inefficient and difficult to scale. Therefore, the project aims to leverage data science techniques to systematically extract sentiment patterns, engagement behaviour, and recurring complaint themes from Google Play Store reviews.

This project follows the CRISP-DM framework, progressing from business understanding to modelling and evaluation to ensure structured and reliable analytics development.

My assigned responsibility was **Objective 2 – Sentiment Quantification and Pattern Analysis**.

**Business Objective:**  
To quantify user sentiment across brands and develop a predictive text classification model capable of automatically identifying dissatisfied customers based on review text. The goal is to support scalable monitoring and early detection of recurring service or operational issues.

---
 
## Work Accomplished

### 1. Dataset Source

- Platform: Google Play Store
- Dataset: Sportswear App Reviews
- Brands: Nike, Adidas, Puma, Gymshark
- Source: Kaggle – *sportswear-app-reviews-google-play*  
  https://www.kaggle.com/datasets/krisbruurs/sportswear-app-reviews-google-play

The raw dataset contains the following fields:
- brand – Application brand
- review_id – Unique review identifier
- score – Star rating (1–5)
- at – Review timestamp
- content – Review text
- reply_content – Developer reply (largely missing)
- thumbs_up – Number of users who found the review helpful
- review_created_version – App version at time of review

### 2. Data Understanding

#### Dataset Overview

- 6,446 total reviews
- 4 brands (Nike, Adidas, Puma, Gymshark)
- No duplicate rows detected

#### Rating Distribution
 
- 5-star reviews ≈ 66%
- Overall positive (4–5 stars) ≈ 74%
- Neutral ≈ 4%

This indicates strong class imbalance. This imbalance significantly influences model evaluation and selection.

#### Engagement Distribution

Thumbs-up counts are highly right-skewed:
- Most reviews receive zero engagement
- A small number of negative reviews receive disproportionately high thumbs-up counts

This suggests that complaints resonate strongly with other users.

#### Short Review Insight
Approximately 40% of reviews contain three words or fewer, limiting contextual richness and increasing classification uncertainty.


### 3. Data Preparation, Engineering & Feature Selection

The following cleaning and engineering steps were performed:

#### Structural Cleaning

- Backed up raw dataset
- Standardised column names
- Renamed at → review_datetime
- Converted to datetime format
- Created year_month feature for trend analysis
- Dropped reply_content (93% missing values)
- Validated score range (1–5 only)
- Validated brand values (Nike, Adidas, Puma, Gymshark)
 
#### Data Quality Validation

- No duplicate rows detected
- Score range validated (1–5 only)
- Brand values validated (Nike, Adidas, Puma, Gymshark)
- reply_content dropped due to 93% missing values
- 118 rows removed after preprocessing due to empty clean_content (fully cleaned text contained no valid tokens)

#### Fields Used for Analysis & Modelling

The following variables were retained for analysis:
- brand
- score
- content
- thumbs_up
- review_datetime (renamed from at)
- review_created_version

The reply_content column was removed due to 93% missing values and limited analytical relevance.

#### Engineered Features

The following features were engineered during Data Preparation:

- review_datetime (converted "at" string data type to datetime format)
- year_month (for temporal sentiment trend analysis)
- content_length_chars (review length in characters)
- content_length_words (review length in words)
- clean_content (fully preprocessed text for modelling)
- sentiment_label (rating-derived target variable)
 
#### Text Preprocessing Pipeline

Preprocessing applied to create clean_content:
- Lowercasing and whitespace normalisation
- Normalisation of negations (e.g., "can't" → "cant")
- Removal of punctuation, numbers, emojis, and non-ASCII characters
- Stopword removal (while preserving negation words such as "not", "never")
- Tokenisation using NLTK
- No stemming (to preserve word clarity)

Note: Removal of non-ASCII characters may reduce representation of multilingual content.

#### Sentiment Ground Truth Construction

Sentiment labels were derived using rule-based mapping from ratings:
- 1–2 → Negative
- 3 → Neutral
- 4–5 → Positive

These rating-derived labels serve as ground truth to evaluate whether textual review content can predict sentiment automatically.

#### Final Dataset for Analysis
After preprocessing and removal of empty `clean_content` records, 
the final dataset used for pattern analysis and modelling contains 
6,328 reviews with 12 features.

The sentiment distribution below reflects this cleaned dataset.

<p align="center">
  <img src="/assets/Images/sentiment-label-distribution.jpg" width="500">
</p>

### 4. Pattern Analysis

Exploratory analysis included:
- Sentiment distribution by brand
  <p align="center">
  <img src="/assets/Images/distribution-by-brand.jpg" width="500">
  </p>
- Engagement behaviour (thumbs-up vs sentiment)
 <p align="center">
  <img src="/assets/Images/thumbs-up-by-sentiment-deistribution.jpg" width="500">
  </p>
- Monthly sentiment trend analysis
 <p align="center">
  <img src="/assets/Images/monthly-sentiment-trend.jpg" width="500">
  </p>
- Word frequency analysis
<p align="center">
  <img src="/assets/Images/unigram.jpg" width="500">
  </p>
- Bigram analysis
  <p align="center">
  <img src="/assets/Images/bigram.jpg" width="500">
  </p>

Key Findings:

- **Brand-Level Sentiment Distribution**
    - Adidas shows the highest proportion of negative reviews.
    - Puma shows the highest proportion of positive reviews.
    - Overall sentiment is predominantly positive (≈74%).

- **Engagement Behaviour**
    - Negative reviews receive the highest average thumbs-up engagement.
    - High-engagement complaints suggest shared user frustrations and act as early-warning signals.

- **Temporal Trend Analysis**
    - Sentiment remains largely stable across most months.
    - A slight increase in negative sentiment is observed toward 2025.
    - Early-month fluctuations are due to smaller review volumes.

- **Text Pattern Analysis (Bigram Findings)**
    - Complaint themes: customer service, refund, delivery, login issues.
    - Success drivers: easy use, easy navigation, product quality.
    - Operational issues dominate negative sentiment themes.

### 5. Modelling & Evaluation

#### Evaluation Strategy
Due to strong class imbalance, overall accuracy was not treated as the primary evaluation metric.

Instead:
- Macro F1-score was used to ensure balanced performance across classes.
- Balanced accuracy was used to mitigate majority-class bias.
- Confusion matrices were analysed to inspect misclassification patterns.
- Stratified train-test split was applied.

#### 3-Class Baseline Models
Models evaluated:
- BoW + Naive Bayes
- BoW + Logistic Regression
- TF-IDF + Naive Bayes
- TF-IDF + Logistic Regression
- TF-IDF + SVM (LinearSVC)

**BoW + Naive Bayes** achieved the highest Macro F1-score and overall balanced performance among baseline models.

However:
- Neutral recall remained consistently low (~4% class share).
- Positive class dominance affected model balance.

#### Hyperparameter Tuning (Improvement Attempt)

TF-IDF + SVM was selected for hyperparameter tuning using GridSearchCV.

Tuned parameters included:
- min_df
- max_df
- sublinear_tf
- C
- class_weight

Although tuning slightly improved Neutral recall, it did not surpass BoW + Naive Bayes in overall Macro F1-score. 

#### Baseline vs Tuned Comparison

Despite tuning TF-IDF + SVM, the baseline BoW + Naive Bayes model achieved higher Macro F1-score in the 3-class setting.

This indicates that model complexity does not necessarily outperform simpler probabilistic approaches under severe class imbalance conditions.


#### Model Improvement – Binary Redesign - Dissatisfaction Detection

Due to persistent Neutral class imbalance and low recall performance (~4% class share), a binary redesign was implemented to prioritise reliable dissatisfaction detection over full sentiment granularity.

Final classification:
- Negative
- Positive

The dataset remained imbalanced even after removing the Neutral class; therefore:
- Class-weighted models were applied
- Balanced evaluation metrics were prioritised

Models evaluated:
- TF-IDF + Logistic Regression (class_weight="balanced")
- TF-IDF + SVM (tuned)
 
#### Final Model Selected

**TF-IDF (Unigram) + Logistic Regression (class_weight="balanced")**

Rationale:

- Highest F1-score for Negative class
- Best balanced accuracy
- Strong alignment with business monitoring needs
- More stable minority-class detection

This model provides a practical balance between interpretability, computational efficiency, and minority-class performance.

---

## Recommendation and Analysis

###  Key Analytical Findings:

- 74% of reviews are positive (4–5 stars), indicating overall satisfaction.
- Neutral class represents only ~4%, creating modelling difficulty.
- 40% of reviews are ≤3 words, limiting contextual signal.
- Negative reviews receive disproportionately higher thumbs-up engagement.
- Complaint themes focus on operational reliability, particularly customer service responsiveness, refund and delivery issues, login errors, and sync failures.

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
