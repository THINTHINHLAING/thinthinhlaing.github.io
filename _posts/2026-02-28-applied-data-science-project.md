---
layout: post
author: Thin Thin Hlaing
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background

Our team project focuses on enhancing sportswear mobile application user experience through data-driven review analytics. Major sportswear brands such as Nike, Adidas, Puma, and Gymshark rely heavily on user reviews to monitor satisfaction, identify operational issues, and improve product quality.

However, manually analysing thousands of user reviews is inefficient and difficult to scale. Therefore, the project aims to leverage data science techniques to systematically extract sentiment patterns, engagement behaviour, and recurring complaint themes from Google Play Store reviews.

This project follows the CRISP-DM framework, progressing from business understanding to modelling and evaluation to ensure structured and reliable analytics development.

My assigned responsibility was **Objective 2 – Sentiment Quantification and Pattern Analysis**.

**Business Objective:**  
To quantify user sentiment across brands and develop a predictive text classification model capable of automatically identifying dissatisfied customers based on review text. The goal is to support scalable monitoring and early detection of recurring service or operational issues.


**Key Achievements**

- Developed a sentiment classification pipeline using BoW, TF-IDF and supervised machine learning.
- Reduced negative misclassification rate to 7.1% through class-weighted modelling.
- Achieved 0.930 balanced accuracy in binary dissatisfaction detection.
- Identified high-engagement complaint patterns linked to operational issues.
- Provided actionable business recommendations for early dissatisfaction monitoring.

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

#### Rating Score Distribution
 <p align="center">
  <img src="/assets/Images/original-score-distribution.jpg" width="500">
</p>

- 5-star reviews ≈ 66%
- Positive ratings (4–5 stars) ≈ 74%
- 3-star (Neutral) reviews ≈ 4%

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
- Created text length features (character and word count)
- Created year_month feature for trend analysis
- Dropped reply_content (93% missing values)
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

#### Sentiment Ground Truth Construction

Sentiment labels were derived using rule-based mapping from ratings:
- 1–2 → Negative
- 3 → Neutral
- 4–5 → Positive

These rating-derived labels serve as ground truth to evaluate whether textual review content can predict sentiment automatically.

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

####  Cleaned Dataset for Analysis and 3-Class Baseline Models
After preprocessing and removal of empty clean_content records (n = 118), the final dataset used for pattern analysis and modelling contains 6,328 reviews with 12 features.

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
  <img src="/assets/Images/bigram.jpg" width="600">
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
    - Early-period fluctuations are likely influenced by smaller review volumes.

- **Text Pattern Analysis (Bigram Findings)**
    - Complaint themes: customer service, refund, delivery, login issues.
    - Success drivers: easy use, easy navigation, product quality.
    - Operational issues dominate negative sentiment themes.

### 5. Modelling & Evaluation

## Phase 1 – 3-Class Sentiment Classification (Presented in Final Presentation)

### 5.1 Model Setup & Configuration

**Objective:**  
Train a supervised text classification model to predict user sentiment from review text.

**Target Variable (y):** `sentiment_label`  ( 1–2 → Negative | 3 → Neutral | 4–5 → Positive  )

**Input Feature (X):**  `clean_content` (preprocessed review text)

**Text Representation Techniques:**
- Bag-of-Words (BoW)
- TF-IDF (Unigram)

**Train–Test Strategy:**
- 80% Training
- 20% Testing
- Stratified sampling (preserve class distribution)
 
### 5.2 Evaluation Strategy (Handling Class Imbalance)

The dataset is highly imbalanced:
- 74% Positive
- 22% Negative
- 4% Neutral

Accuracy alone may overestimate model performance due to majority-class dominance.

**Primary Evaluation Metric:**
- Macro F1-score (equal importance across classes)

**Additional Metrics:**
- Weighted F1-score
- Neutral Recall (minority class)
- Balanced Accuracy
- Confusion Matrix analysis

### 5.3 3-Class Baseline Models Evaluated

- BoW + Naive Bayes
- BoW + Logistic Regression
- TF-IDF + Naive Bayes
- TF-IDF + Logistic Regression
- TF-IDF + SVM (LinearSVC)

**Result:**  BoW + Naive Bayes achieved the highest Macro F1-score (0.651) and demonstrated the most balanced overall performance among baseline models.

However:
- Neutral recall remained low (~4% class share)
- Positive class dominance influenced predictions

## Phase 2 – Hyperparameter Tuning (Improvement Attempt)

### 5.4 TF-IDF + SVM Tuning

To explore potential performance improvement, TF-IDF + LinearSVC was selected for tuning using GridSearchCV.

**Tuned Parameters:**
- `C`
- `class_weight`
- `min_df`
- `max_df`
- `sublinear_tf`

Although tuning improved Neutral recall slightly, it did not surpass the baseline BoW + Naive Bayes model in overall Macro F1-score.

### 5.5 Final 3-Class Model (Presented Version)

**Selected Model:**  
Bag-of-Words + Naive Bayes

**Reason for Selection:**
- Highest Macro F1-score (0.651)
- Balanced performance across classes
- Computationally efficient
- Interpretable and scalable

This was the model selected and presented during the final project presentation.

## Phase 3 – Deployment Limitation Identified (Examiner Feedback)

During the final presentation, feedback highlighted that:

- The model showed strong majority-class (Positive) dominance
- Confusion matrix patterns indicated limited operational focus on dissatisfied users
- The model may not be sufficiently reliable for real-world deployment

From a business perspective, reliable identification of dissatisfied users is more critical than full 3-class granularity.

This motivated a reformulation of the modelling objective.

## Phase 4 – Binary Redesign (Dissatisfaction Detection)

### 5.6 Binary Redesign – Problem Reformulation & Model Setup

During the final presentation, feedback highlighted that the 3-class model, although optimal under Macro F1-score, showed majority-class dominance and limited operational focus on dissatisfied users.

To improve deployment suitability, the modelling objective was reformulated from 3-class sentiment classification into binary dissatisfaction detection.

**New Target Variable (y):**
- Negative → Dissatisfied
- Positive → Satisfied
- Neutral class removed

<p align="center">
  <img src="/assets/Images/binary-sentiment-label-distribution.jpg" width="500">
</p>

After removing the Neutral class, the dataset remained moderately imbalanced:

- 76.8% Positive  
- 23.2% Negative  

**Input Feature (X):**
- `clean_content` (same preprocessed review text)

**Train–Test Strategy:**
- 80% Training  
- 20% Testing  
- Stratified sampling applied  

**Imbalance Handling Strategy:**
- Applied `class_weight="balanced"` in linear models  
- Prioritised Balanced Accuracy and Negative Recall  
- Analysed False Negative Rate to minimise missed dissatisfied users  

This redesign shifts the modelling focus from general sentiment categorisation to reliable identification of dissatisfied users for real-world monitoring.

### 5.7 Binary Models Evaluated

- TF-IDF + Logistic Regression (`class_weight="balanced"`)
- TF-IDF + LinearSVC (`class_weight="balanced"`)
- TF-IDF + Complement Naive Bayes

 <p align="center">
  <img src="/assets/Images/final-comparison-table.jpg" width="500" >
  </p>
  
Among evaluated models, **TF-IDF (Unigram) + Logistic Regression** achieved:

- Negative F1-score = 0.861
- Negative Recall = 0.929
- Balanced Accuracy = 0.930

### 5.8 Final Improved Model (Deployment-Oriented)

**TF-IDF (Unigram) + Logistic Regression (class_weight="balanced")**

  <p align="center">
  <img src="/assets/Images/final-selection-model.jpg" width="500">
  </p>
  
**False Negative Rate:**  20 / (261 + 20) ≈ 7.1%

This means only 7.1% of dissatisfied users would be missed.

## Final Modelling Conclusion

The modelling process evolved through:

1. 3-class baseline exploration  
2. Hyperparameter tuning  
3. Deployment feasibility evaluation  
4. Business-driven binary redesign  

While the 3-class BoW + Naive Bayes model was optimal under Macro F1 evaluation, the binary redesign provided stronger operational reliability for dissatisfaction detection.

This demonstrates an iterative, business-aligned modelling approach consistent with the CRISP-DM framework.

---

## Recommendation and Analysis

###  Key Analytical Findings:

- 74% of reviews are positive (4–5 stars), indicating overall satisfaction.
- Neutral class represents only ~4%, creating modelling difficulty.
- 40% of reviews are ≤3 words, limiting contextual signal.
- Negative reviews receive disproportionately higher thumbs-up engagement.
- Complaint themes focus on operational reliability, particularly customer service responsiveness, refund and delivery issues, login errors, and sync failures.

### Business Recommendations

Following the binary redesign, recommendations focus specifically on reliable dissatisfaction detection (Negative vs Positive classification) to support early operational intervention.

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

This project demonstrates the practical application of supervised text classification for scalable business-oriented dissatisfaction detection under real-world class imbalance conditions.

---
  
## Source Codes and Datasets
Project Repository:
 - https://github.com/THINTHINHLAING/ITD214-Sportswear-App-Review-Analytics.git
