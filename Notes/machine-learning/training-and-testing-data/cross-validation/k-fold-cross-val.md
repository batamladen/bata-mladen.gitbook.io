# K-Fold Cross Val

## What is K-fold

**k-Fold Cross-Validation** is one of the most commonly used types of cross-validation. It involves the following steps:

1. **Splitting the Data**: The entire dataset is divided into `k` equal (or nearly equal) parts, known as folds.
2. **Training and Testing**:
   * For each fold:
     * The model is trained on `k-1` of the folds (combined into a training set).
     * The model is tested on the remaining fold (the validation set).
3. **Repeating the Process**: This process is repeated `k` times, with each fold used exactly once as the test set.
4. **Averaging the Results**: The results from each of the `k` iterations are averaged to produce a single performance metric.

***

## Example

```python
from sklearn.model_selection import cross_val_score
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris

# Load data
iris = load_iris()
X, y = iris.data, iris.target

# Create model
model = RandomForestClassifier()

# Perform 5-fold cross-validation
scores = cross_val_score(model, X, y, cv=5)
print("Cross-validation scores:", scores)
print("Mean cross-validation score:", scores.mean())

```
