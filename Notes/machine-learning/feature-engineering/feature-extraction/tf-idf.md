# TF-IDF

TF-IDF is a [**feature extraction**](./) method. TF-IDF is used on textual data to transform it into numbers, so that the model can understand it.\
TF-IDF stands for Term Frequency - Inverse Document Frequency

**Term frequency** represents the amount of times a word appears in a document. This can be represented like this:

```
tf(t,d) = count of t in d / number of words in d
```



**Document Frequency:** This tests the meaning of the text, which is very similar to TF, in the whole corpus collection. The only difference is that in document d, TF is the frequency counter for a term t, while df is the number of occurrences in the document set N of the term t.

```
df(t) = occurrence of t in documents
```



**Inverse Document Frequency:** Mainly, it tests how relevant the word is. The key aim of the search is to locate the appropriate records that fit the demand.

First, find the document frequency of a term t by counting the number of documents containing the term:

```
df(t) = N(t)
where
df(t) = Document frequency of a term t
N(t) = Number of documents containing the term t
```

Now let’s look at the definition of the frequency of the inverse paper. The IDF of the word is the number of documents in the corpus separated by the frequency of the text.

```
idf(t) = N/ df(t) = N/N(t)

```

We then take the logarithm (with base 2) of the inverse frequency of the paper. So the if of the term t becomes:

```
idf(t) = log(N/ df(t))
```

Usually, the tf-idf weight consists of two terms-

1. **Normalized Term Frequency (tf)**
2. **Inverse Document Frequency (idf)**

```
tf-idf(t, d) = tf(t, d) * idf(t)
```

***

## Syntax

{% tabs %}
{% tab title="Import" %}
```python
from sklearn.feature_extraction.text import TfidfVectorizer
```
{% endtab %}

{% tab title="Create obj" %}
```
tfidf = TfidfVectorizer()
```
{% endtab %}

{% tab title="Fit" %}


```python
# get tf-df values
X_new = tfidf.fit_transform(string)
```
{% endtab %}
{% endtabs %}

***

## Example
