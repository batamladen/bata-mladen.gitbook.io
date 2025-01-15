# Feature engineering

## What is feature engineering?

Feature engineering is the process of creating new features or modifying existing features in a dataset to improve the performance of a machine learning model. This step is crucial because the quality and relevance of the features used in the model can significantly impact its ability to learn and make accurate predictions. Feature engineering involves several techniques.

***

## Feature engineering techniques

* Feature Selection - KBest
* Dimensionality Reduction - SVD
* Feature Extraction - TF-IDF(changing text features to number so they can be used in modules)
* Feature Scaling
* Feature Encoding
* Feature Creation

## Are normalization & standardization the same thing?

Normalization and standardization are similar in the sense that they both involve **rescaling the features of a dataset**. However, they are not the same thing and serve different purposes.

**Formula:** $$Xnorm = \frac{X - X_{\text{min}}}{X_{\text{max}} - X_{\text{min}}}​​$$

**Formula:** $$Z= \frac{(X - \mu)}{\sigma}$$
