# Random Forest

## What is Random Forest?

Random forest is an [ensemble learning](https://en.wikipedia.org/wiki/Ensemble_learning) method/technique for [classification](https://en.wikipedia.org/wiki/Statistical_classification) and [regression](https://en.wikipedia.org/wiki/Regression_analysis) tasks that operates by constructing a multitude of [decision trees](https://en.wikipedia.org/wiki/Decision_tree_learning) at training time.&#x20;

For **classification tasks**, the output of the random forest is the class selected by most trees.\
For **regression tasks**, the mean or average prediction of the individual trees is returned.

***

### Random Forest Classifier

#### Importing:

```python
from sklearn.ensemble import RandomForestClassifier

```

#### Training the module:

```python
clf = RandomForestClassifier(random_state=42)
# Creating a Random Forest classifier object

clf.fit(X_train, y_train)
# Training the classifier on the training data
```

#### Making predictions:

```
y_pred = clf.predict(X_test)
```

