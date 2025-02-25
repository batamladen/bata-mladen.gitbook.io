# Classification Raport

## What is a classification raport?

A classification report is a performance evaluation tool used in machine learning to assess the quality of predictions from a classification algorithm.

The classification report typically includes the following metrics:

* **Precision**: The ratio of true positive predictions to the total predicted positives. It answers the question, "Of all the instances predicted as positive, how many are actually positive?"
* **Recall (Sensitivity or True Positive Rate)**: The ratio of true positive predictions to the total actual positives. It answers the question, "Of all the actual positive instances, how many were correctly predicted?"
* **F1 Score**: The harmonic mean of precision and recall, providing a single metric that balances the two. It is particularly useful when you need a balance between precision and recall.
* **Support**: The number of actual occurrences of each class in the dataset. It helps in understanding the distribution of the dataset.

***

## **Calculating Model Accuracy**

Accuracy is calculated by dividing the number of correct predictions by the total number of predictions across all classes. In binary classification, it can be expressed as:

Accuracy (ACC) = (TP + TN) / (TP + TN + FP + FN)

Where:

* TP: True Positives (correctly predicted positive instances)
* TN: True Negatives (correctly predicted negative instances)
* FP: False Positives (negative instances predicted as positive)
* FN: False Negatives (positive instances predicted as negative)

***

## Classification report example

In order to use the accuracy and the rest of the metrics, we need to import the class

#### import classification report and accuracy:

```python
from sklearn.metrics import accuraccy, classification_report
```

#### define class lables:

```python
target_names = ['ham', 'spam'] 
```

#### Print accuracy:

```python
accuracy = accuracy_score(y_test, y_pred)
print("Accuracy: " + accuracy)

# Output:
# Accuracy: 0.9999415592005299
```

#### Print the report:

```python
report = classification_report(y_test, y_pred, target_names=target_names)
print(report)

# Output:

#                 precision    recall  f1-score   support
#
#              0       1.00      1.00      1.00     51126
#              1       0.99      1.00      0.99       208

#       accuracy                           1.00     51334
#      macro avg       0.99      1.00      1.00     51334
#   weighted avg       1.00      1.00      1.00     51334
```

***

## Full report example:

```python
from sklearn.metrics import accuracy_score, classification_report

target_names = ["Class 1", "Class 2", "Class 3"]

accuracy = accuracy_score(y_test, y_predicted)
report = classification_report(y_test, y_predicted, target_names=target_names)

print(accuracy)
print(report)
```

