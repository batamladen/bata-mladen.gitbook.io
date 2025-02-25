# Data Exploration

## **What is data exploration?**

**Data exploration** refers to the initial step in data analysis in which we use data visualization and statistical techniques to describe dataset characterizations, such as <mark style="color:purple;">size</mark>, <mark style="color:purple;">quantity</mark>, and <mark style="color:purple;">accuracy</mark>, in order to better understand the nature of the data.

Key aspects of data exploration include:

1. **Understanding the Data Structure**:
   * Examining the types of data (e.g., numerical, categorical, datetime).
   * Checking for missing values and handling them appropriately.
   * Understanding the distribution of the data, including the mean, median, mode, variance, and standard deviation.
2. **Data Visualization**:
   * Creating visual representations of the data to uncover underlying patterns.
   * Using plots like histograms, bar charts, box plots, scatter plots, and heatmaps.
   * Visualizing correlations between variables using correlation matrices and pair plots.

***

## Normal Distribution

The normal distribution os a theoretical concept of how large samples of ratio or interval level data will look once its plotted.

In a normal distribution, measures of central tendency are: mean, meadinan, mode.

* **Mean**: The average of the data.
* **Median**: The middle value separating the higher half from the lower half of the data.
* **Mode**: The most frequently occurring value in the data.

***

## Standard Deviation

Standard deviations are used to measure how much variation exists in a distribution.

Low standard deviations means that the values are close to the [<mark style="color:purple;">mean</mark>](#user-content-fn-1)[^1]\
High standard deviation means that values are spread over a large range.

For us to understand more about distributions it is important to understand modality, symmetry and peakedness.

***

## Modality

A distributions can have more than one peak. The number of peaks in a distribution determines the **modality** of the distribution

<figure><img src="../.gitbook/assets/image (28).png" alt="" width="167"><figcaption></figcaption></figure>

Most distribution are normally distriburted and have only one peak, however it is possible to have distibutions with 2 or more peaks.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

***

## Skewness

If two halves of a distibution can be superimposed on each other, where one i a mirror of the other, the data is symetrical.

However sometimes data is not symetrical. If the peak is not in the center, one tail of the distribution will be longer than the other. Meaning it is **skewed.**

Skewness is a measure of symetry of distributions

$$
Skewness = \frac{\text{Mean} - \text{Median}}{\text{Standard Deviation}}
$$

In a perfect world the skewness would be zero, because the mean = median.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption><p>skewness = 0</p></figcaption></figure>

Positive skewness means there is a lot of data to the left\
Negative skewness means there is a lot of data on the right

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

***

## Kurtosis

Kurtosis measures if the bell of the curve is normal, flat or peaked

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption><p>Kurtosis and its 3 types</p></figcaption></figure>



[^1]: The average of the data
