# SelectKBest

## What is SelectKBest? <a href="#a64d" id="a64d"></a>

**SelectKBest** is one of the most commonly used feature selection methods.\
SelectKBest is a type of filter-based feature selection method in machine learning.

SelectKBest uses statistical tests like chi-squared test, ANOVA F-test, or mutual information score to score and rank the features based on their relationship with the output variable. Then, it selects the K features with the highest scores to be included in the final feature subset.

***

## Syntax

```
SelectkBest = SelectKBest(f_classif, k=3)
```

**SelectKBest has 2 parameters: score function & number of fetures(k)**

***

## **Score function**

**Score function** is used to evaluate the feature importance. We have different types of score functions.

Some of the commonly used `score_func` functions in `SelectKBest`:

1. <mark style="color:orange;">`f_regression`</mark>: It is used for linear regression problems and computes F-value between feature and target.
2. <mark style="color:orange;">`mutual_info_regression`</mark>: It is used for regression problems and computes mutual information between two random variables.
3. <mark style="color:orange;">`f_classif`</mark>: It is used for classification problems and computes ANOVA F-value between feature and target.
4. <mark style="color:orange;">`mutual_info_classif`</mark>: It is used for classification problems and computes mutual information between two discrete variables.
5. <mark style="color:orange;">`chi2`</mark>: It is used for classification problems and computes chi-squared statistics between each feature and target.
6. <mark style="color:orange;">`SelectPercentile`</mark>: It is used to select the highest X% of the features based on the score\_func.



### How to select the right score function?

For regression, the most commonly used scoring functions are `f_regression` and `mutual_info_regression`

For classification, the most commonly used scoring function is `chi_2`**,** `mutual_info_classif` and `f_classif`

<details>

<summary>chi_2</summary>

`chi_2`: It is used to test the independence between two categorical variables. In feature selection, it computes the chi-squared statistic between each feature and the target variable. Features that are highly correlated with the target variable will have higher scores.

</details>

<details>

<summary>mutual_info_classif</summary>

`mutual_info_classif`: It is based on the concept of mutual information, which measures the amount of information shared between two variables. It computes the mutual information between each feature and the target variable. Features that are highly informative with respect to the target variable will have high scores.

</details>

<details>

<summary>f_classif</summary>

`f_classif`: It is based on ANOVA (analysis of variance). It computes the F-value between each feature and the target variable, which measures the linear dependency between two variables. Features that are highly dependent on the target variable will have high scores.

</details>



***

## Commands

{% tabs %}
{% tab title="Import skb" %}
```python
from sklearn.feature_selection import SelectKBest
```
{% endtab %}

{% tab title="Import regression" %}
```python
from sklearn.feature_selection import f_regression
```
{% endtab %}

{% tab title="Regression object" %}
```
X_new = SelectKBest(f_regression, k=2)
```

<mark style="color:orange;">f\_regression</mark> defines that we are making a regression model.
{% endtab %}

{% tab title="Amount of fetures to select" %}
```
X_new = SelectKBest(f_regression, k=2)
```

<mark style="color:orange;">k=2</mark> defines that we want 2 features to use from the dataframe, the algorithm will decide whitch will it be&#x20;
{% endtab %}

{% tab title="Transform data" %}
```
X_new = SelectKBest(f_regression, k=2).fit_transform(X_train, y_train)
```

<mark style="color:orange;">.fit\_transform(X\_train, y\_train)</mark> trains the data from the X\_train, y\_train splited dataframes and stores them in `X_new` in this case.
{% endtab %}
{% endtabs %}

