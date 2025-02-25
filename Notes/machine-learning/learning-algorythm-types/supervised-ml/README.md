# Supervised ML

## **What is Supervised ML?**

**Supervised Machine Learining** involves a series of functions that map an input to an output, based on a series of input → outpt fails.

So the process typically involves:

1. **Training Data**: This is the dataset used to train the model. It includes both the input features and the corresponding target labels. The model learns from this data to make predictions.
2. **Testing Data**: This dataset is used to evaluate the performance of the model. It includes input features but no target labels during the training process. After training, the model makes predictions on the test data, and these predictions are compared to the actual labels to assess accuracy, precision, recall, and other performance metrics.

{% content-ref url="../../training-and-testing-data/train-test-split.md" %}
[train-test-split.md](../../training-and-testing-data/train-test-split.md)
{% endcontent-ref %}

Fore example with this model we could predict a shoe size for a child that is 10yo, based on the data below:

<table><thead><tr><th width="356">Age</th><th>Shoe Size</th></tr></thead><tbody><tr><td>7</td><td>19.5</td></tr><tr><td>8</td><td>21</td></tr><tr><td>9</td><td>22.3</td></tr><tr><td>11</td><td>24.8</td></tr></tbody></table>

***

## Types of supervised learning:

1. Regression
2. Classification

***

### Regression

In regression, we aim to predict real values based on the features present in the training data. Unlike classification, where we assign classes, regression focuses on estimating continuous and numerical values.

#### Regression algorithms:&#x20;

* Linear regression
* Logistic regression
* Decision Tree Regression
* Random Forest Regression
* Ridge regression
* Lasso regression

***

### Classification

Classification is a type of supervised learning technique in machine learning (ML) used for predicting categorical outcomes. Unlike regression, which deals with predicting continuous numerical values, classification models are designed to assign input data to one of several predefined categories or classes.

#### **Types of Classification**:

* **Binary Classification**: The target variable has two possible classes (e.g., spam/not spam, disease/healthy).
* **Multiclass Classification**: The target variable has more than two classes (e.g., classifying types of animals, predicting digit values).
* **Multilabel Classification**: Each instance can belong to multiple classes simultaneously (e.g., tagging an image with multiple objects).

#### Classification algorithms:

* Decision Trees Classification
* [Random Forest](random-forest.md) Classification
* K-Nearest Neighbors
* Support Vector Machines (SVM)
* Neural Networks
