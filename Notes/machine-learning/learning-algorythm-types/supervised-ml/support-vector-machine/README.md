---
description: >-
  Feature: supervised ; Common use: classification ; Output: class label ; Key
  Parameters: C(regularization), kernel, gamma
---

# Support Vector Machine

**Support vector machines (SVMs)** are a set of supervised learning methods used for [classification](https://scikit-learn.org/stable/modules/svm.html#svm-classification), [regression](https://scikit-learn.org/stable/modules/svm.html#svm-regression) and [outliers detection](https://scikit-learn.org/stable/modules/svm.html#svm-outlier-detection).

The <mark style="color:green;">advantages</mark> of support vector machines are:

* Effective in high dimensional spaces.
* Still effective in cases where number of dimensions is greater than the number of samples.
* Uses a subset of training points in the decision function (called support vectors), so it is also memory efficient.
* Versatile: different [Kernel functions](https://scikit-learn.org/stable/modules/svm.html#svm-kernels) can be specified for the decision function. Common kernels are provided, but it is also possible to specify custom kernels.

The <mark style="color:red;">disadvantages</mark> of support vector machines include:

* If the number of features is much greater than the number of samples, avoid over-fitting in choosing [Kernel functions](https://scikit-learn.org/stable/modules/svm.html#svm-kernels) and regularization term is crucial.
* SVMs do not directly provide probability estimates, these are calculated using an expensive five-fold cross-validation (see [Scores and probabilities](https://scikit-learn.org/stable/modules/svm.html#scores-probabilities), below).



***

## What does SVM do?

SVM finds the **best boundary (hyperplane)** that separates classes **with the maximum margin**.

* For 2D data, this boundary is a line.
* For 3D+, it's a hyperplane.
* The **support vectors** are the data points closest to this boundary and most critical in defining it.



***

## Key Concepts

| Term                | Meaning                                                                                                     |
| ------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Hyperplane**      | Decision boundary that separates the classes                                                                |
| **Margin**          | Distance between the hyperplane and the closest support vectors                                             |
| **Support Vectors** | Data points closest to the hyperplane that influence its position                                           |
| **Kernel Trick**    | Technique that transforms data into a higher dimension to make it separable when it's not in original space |



***

## Types of Kernels:

Used to handle **non-linear** problems:

* **Linear**: Default, used when data is linearly separable.
* **Polynomial**: Useful for curved boundaries.
* **RBF (Radial Basis Function)** / **Gaussian**: Most common for non-linear data.



***

## SVC (Support Vector Classification)

**SVC** is the **classification implementation of Support Vector Machine (SVM)** in **Scikit-learn**, the popular Python machine learning library.



***

## Usage

```python
from sklearn.svm import SVC
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load sample data
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Create SVC model
model = SVC(kernel='linear')  # other options: 'rbf', 'poly'
model.fit(X_train, y_train)

# Make predictions
y_pred = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))

```





