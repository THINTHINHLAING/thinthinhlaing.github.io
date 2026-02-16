---
layout: post
author: THIN THIN HLAING
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background

<p align="justify">
Mobile applications are a critical customer touchpoint for global sportswear brands such as Nike, adidas, PUMA, and Gymshark. These apps support product browsing and purchases, but they also influence customer engagement and loyalty. When users face issues such as crashes, slow performance, login problems, or poor usability, they may leave low ratings and negative reviews, which can reduce user retention and harm brand perception.
</p>

<p align="justify">
Google Play Store reviews contain rich customer feedback in both structured form (ratings, timestamps, thumbs-up counts) and unstructured form (review text). However, the large volume and unstructured nature of reviews make manual analysis inefficient and may miss important patterns. Therefore, a systematic and data-driven approach is needed to extract insights that can support app improvement decisions.
</p>

<p align="justify">
This project analyses 6,446 publicly available and anonymised Google Play Store reviews collected from Kaggle. Each review includes brand, star rating (1–5), review text, timestamp, and engagement indicators, enabling both text analytics and predictive modelling.
</p>

<p align="justify"> **Business Goal** </p>
<p align="justify">
The business goal of this project is:
- To enhance user satisfaction and engagement for sportswear mobile applications (Nike, adidas, PUMA, and Gymshark) by leveraging data-driven insights derived from Google Play Store reviews.
</p>

<p align="justify"> **Project Objectives**</p>
<p align="justify">
To support the business goal, the team defined three objectives:
- **Objective 1**: Identify key themes and recurring discussion topics across brands using topic modelling.
- **Objective 2 (My Responsibility)**: Quantify user sentiment and analyse emotional tone using supervised sentiment classification.
- **Objective 3**: Determine primary drivers of user satisfaction through statistical analysis and predictive modelling.
</p>
 
<p align="justify">For Objective 2, sentiment labels were derived from the "score" column to create a supervised learning framework:</p>
- 1-2 stars → Negative
- 3 stars     → Neutral
- 4-5 stars → Positive

<p align="justify">
A machine learning model was trained to predict sentiment from review text, followed by pattern analysis across brands, time, and engagement indicators to identify factors associated with positive and negative user experiences.
</p>
 
## Work Accomplished
Document your work done to accomplish the outcome

### Data Preparation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

### Modelling
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

### Evaluation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Recommendation and Analysis
Explain the analysis and recommendations

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## AI Ethics

## 1. Privacy & Data Protection
The dataset consists of publicly available user reviews. No personally identifiable information (PII) was collected or used.

## 2. Bias & Class Imbalance
The dataset is imbalanced, with positive reviews representing approximately 74% of data. This may bias models toward majority classes. Macro F1-score was used to address imbalance concerns.

## 3. Transparency
Sentiment labels were generated using explicit rating-based rules, ensuring interpretability and transparency.

## 4. Accountability
Model predictions should support, not replace, human decision-making. Business recommendations derived from sentiment analysis should be validated by domain experts.

## 5. Limitations
- Sarcasm and contextual nuance may not be captured.
- Rule-based sentiment labeling depends solely on rating.
- Review volume varies across time, affecting monthly trend stability.

# Limitations & Future Work

## Limitations
- Sentiment labels were rule-based rather than text-derived.
- Dataset limited to English-language reviews.
- Imbalanced classes affect minority prediction performance.

## Future Improvements
- Apply transformer-based models (e.g., BERT).
- Perform topic modelling for deeper complaint clustering.
- Develop real-time sentiment monitoring dashboard.


## Source Codes and Datasets
Upload your model files and dataset into a GitHub repo and add the link here. 
