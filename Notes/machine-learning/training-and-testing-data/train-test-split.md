# Train-Test Split

For training and testing we import a function called `train_test_split` from `sklearn.model_selection`

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(   
    X, y, test_size=0.2, random_state=0)

# the first 4 parameters are the ones we will get (X_train, X_test, y_train, y_test)
# the arguments in the brackets are the ones we are using (X, y in this case)
# test_size=0.2 means that 20% of the data will be used for test and 80 for training
# different values of random_state will produce different splits, but the same value will always produce the same split.
```

We dont have to use X and y always, we can import columns as well, like here:

```
X_train, X_test, y_train, y_test = train_test_split(
    df[features], df.label, test_size=0.2)
    
# here X is df[features], a column called "features" from the dataframe called df.
# and y is "label" column from df dataframe, just defined another way.
```



Now for training our model we will need to import a class of the module, in our case we will be using random forest.

```python
from sklearn.ensemble import RandomForestClassifier
clf = RandomForestClassifier(random_state=42)
clf.fit(X_train, y_train)
y_predicted = clf.predict(X_test)

# import the classifier class and create an object.
# with .fit we acctualy train our model, and in brackets on the .fit we put our training data (X_train, y_train)
# at the end we use .predict method on our trained classifier (clf) using the testing dataset (X_test) to test out model and store the output in y_predicted.
```

and we can check the **accuraccy** of the module by calling the `accuracy_score` and get our classification report by calling `classification_report` both from `sklearn.metrics`

```python
from sklearn.metrics import accuracy_score, classification_report

accuracy = accuracy_score(y_test, y_pred)
report = classification_report(y_test, y_pred))

# in the accuracy we put y tested and y trained data
# in the report we also use y tested and y trained

# If the accuracy is high, great. But that sould not be our main metric. Because lets say we have a module that scans sick people. if in a group of 1000 people (999 healthy & 1 sick) we scan all but miss the 1 that is sick, we would have 99% accuracy but the module would be ussles.
# That is why if your model has 99% accuracy, you want to ask urself is it usseful?
# That is why we use other metrics like recall. In the upper scenario we would get 1000/(0+0)= 0 recall. Witch is a much more realistic metric because it tells us that the module is ussless.
```

{% hint style="info" %}
Find more about accuracy and recall in [**metrics**](../classification-raport.md).
{% endhint %}

***

## Full Train & Test example:

{% file src="../../.gitbook/assets/Train Test.ipynb" %}

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn import datasets
```

```python
X, y = datasets.load_iris(return_X_y=True)
```

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=0)

print ("Train:\n", X_train, y_train)
print ("Train:\n", X_test, y_test)
```

```python
clf = RandomForestClassifier(random_state=42)
clf.fit(X_train, y_train)
y_predicted = clf.predict(X_test)
print(y_predicted)
```
