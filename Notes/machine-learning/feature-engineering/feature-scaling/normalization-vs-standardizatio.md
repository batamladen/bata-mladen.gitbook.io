# Normalization vs Standardizatio

| This method scales the model using minimum and maximum values.  | This method scales the model using the mean and standard deviation.                 |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| When features are on various scales, it is functional.          | When a variable's mean and standard deviation are both set to 0, it is beneficial.  |
| Values on the scale fall between \[0, 1] and \[-1, 1].          | Values on a scale are not constrained to a particular range.                        |
| Additionally known as scaling normalization.                    | This process is called Z-score normalization.                                       |
| When the feature distribution is unclear, it is helpful.        | When the feature distribution is consistent, it is helpful.                         |



***

## FAQs <a href="#faqs" id="faqs"></a>

### 1. Is normalisation and standardisation same?

Standardization is divided by the standard deviation after the mean has been subtracted. Data is transformed into a range between 0 and 1 by normalization, which involves dividing a vector by its length.

### 2. Why is standardization preferred over normalization?

When the data has a normal distribution, standardization is an excellent tool to use. It can be utilized in a machine learning process when assumptions are made on the distribution of the data, such as in linear regression.

### 3. What is the difference between normalization and scaling?

Changing the range of your data with scaling is different from changing the distribution of your data with Normalization.

### 4. Should I normalize or standardize my data?

When your data have different dimensions and the method you're employing, like k-nearest neighbors or artificial neural networks, doesn't make assumptions about the distribution of your data, normalization is helpful. Standardization presupposes that the distribution of your data is Gaussian.

### 5. Does normalizing improve accuracy?

Your marketing database will be more accurate and contextualized thanks to the systematic process of grouping related information into a common value called data normalization. Data normalization formats your data such that it appears and reads consistently across all database records.

### 6. Which is better, normalization or standardization?

If your feature (column) contains outliers, normalizing your data will scale most of the data to a small interval, ensuring that all components have the same scale but failing to manage outliers adequately. Max-Min Normalization is rarely preferred over standardization since it is less resistant to outliers.\\
