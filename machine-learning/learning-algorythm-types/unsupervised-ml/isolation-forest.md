---
description: Anomaly Detection Algorithm
---

# Isolation Forest

Isolation Forest is an unsupervised machine learning algorithm for **anomaly detection**. As the name implies, Isolation Forest is an ensemble method (similar to random forest). In other words, it use the average of the predictions by several decision trees when assigning the final anomaly score to a given data point. Unlike other anomaly detection algorithms, which first define what’s “normal” and then report anything else as anomalous, Isolation Forest attempts to isolate anomalous data points from the get go.

For consistency with the [`IsolationForest`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html#sklearn.ensemble.IsolationForest) notation, the inliers are assigned a ground truth label `1` whereas the outliers are assigned the label `-1`.
