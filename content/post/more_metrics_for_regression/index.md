---
title: Metrics for Regression - Part 2
date: "2020-02-17T00:00:00"
lastMod: "2020-03-07T00:00:00"
draft: false
math: true
diagram: false
tags: ["Machine Learning", "Regression"]
keywords: ["Machine Learning", "Regression"]
image: 
  placement: 3 
  caption: ''
---

In a [previous post](../metrics_for_regression), we discussed some of the commonly used metrics for Regression. Let us discuss some of the other metrics used : Mean Squared Percentage Error (MSPE), Mean Absolute Percentage Error (MAPE), and Root Mean Squared Logarithmic Error (RMSLE).

## The need for MSPE, MAPE and (R)MSLE

Imagine we are building a ML model to predict trip times for trucks that carry finished products from a factory to a warehouse or to a port. Some of the trips are short (let us say, the destination is less than  100 miles from the origin) and take 2 to 3 hours to complete. And there are longer trips of 1000 miles or more that take more than a day to complete. Let us assume that we have the predicted trip time and the actual time taken for the test data which has 6 samples as shown below:

|Actual Time (mins)|Predicted Time (mins)| Absolute Difference (mins)|
|---:|---:|---:|
|120|200|80
|200|180|20
|220|250|30
|1500|1660|160
|1610|1700|90
|1855|1935|80

 We are interested in the predictions for the 1<sup>st</sup> and the 6<sup>th</sup> sample : the prediction was off by 80 minutes for both of them. But the 1<sup>th</sup> sample was for a short trip of 120 minutes and the 6<sup>th</sup> sample was for a long trip of 1855 minutes. While in absolute terms we were off only by 80 minutes, our model made a  bigger error for the shorter trip than for the longer trip in percentage terms. RMSE or MAE metrics are not affected by whether the prediction error is for a target with a smaller or larger value since they use absolute values. Hence we can use MSPE, MAPE or MSLE as metrics for our regression model since they penalize large errors for smaller inputs by dividing the errors by the target (_in our case $\frac{80}{120} > \frac{80}{1855}$_).

Now, let us look at the equations for the metrics.

## Mean Absolute Percentage Error (MAPE)

MAPE is a weighted version of MAE - the difference between the target value and the predicted value is divided by the target value to account for relative errors.

$$\definecolor{green1}{RGB}{40,162,40} \definecolor{purple1}{RGB}{224,0,224} \definecolor{blue1}{RGB}{116,98,224} \definecolor{orange1}{RGB}{212,106,67} \text{MAPE}\color{blue1}(\color{purple1}y, \color{green1}\hat{y}\color{blue1}) = \color{orange1} \frac{100 \\%}{N} \sum_{1}^{N} {\color{blue1}\left|\frac{\color{purple1} {y}_{i} -\color{green1}{\hat y}_{i}}{\color{purple1} {y}_{i}} \right|}$$

where

$\color{orange1} N \text{ is the number of samples in your evaluation data set}$,

$\color{purple1} {y}_{i} \text{ is the actual value}$ and

$\color{green1} {\hat y}_{i} \text{ is the predicted value}$.

```python
import numpy as np
from sklearn.metrics import mean_absolute_error, mean_squared_error, \
                            mean_squared_log_error

y_true = np.array([120, 200, 220, 1500, 1610, 1855])  
y_pred = np.array([200, 180, 250, 1660, 1700, 1935])

def mean_absolute_percentage_error(y_true, y_pred):
  """implement MAPE since sklearn does not have it"""
  return 100  * np.mean(np.abs((y_true - y_pred) / y_true))

print(f"Mean Absolute Error: {mean_absolute_error(y_true, y_pred):.3f}") 
print(f"Mean Absolute Percentage Error: \
      {mean_absolute_percentage_error(y_true, y_pred):.3f}")  
```

Output:

```plaintext
Mean Absolute Error: 76.667
Mean Absolute Percentage Error: 18.479
```

## Mean Squared Percentage Error (MSPE)

MSPE is a weighted version of MSE - the difference between the target value and the predicted value is divided by the target value to account for relative errors.

$$\definecolor{green1}{RGB}{40,162,40} \definecolor{purple1}{RGB}{224,0,224} \definecolor{blue1}{RGB}{116,98,224} \definecolor{orange1}{RGB}{212,106,67} \text{MAPE}\color{blue1}(\color{purple1}y, \color{green1}\hat{y}\color{blue1}) = \color{orange1} \frac{100 \\%}{N} \sum_{1}^{N} {{\color{blue1}\left(\frac{\color{purple1} {y}_{i} -\color{green1}{\hat y}_{i}}{\color{purple1} {y}_{i}} \right)}}^{\color{blue1}2}$$

where

$\color{orange1} N \text{ is the number of samples in your evaluation data set}$,

$\color{purple1} {y}_{i} \text{ is the actual value}$ and

$\color{green1} {\hat y}_{i} \text{ is the predicted value}$.

```python
def mean_squared_percentage_error(y_true, y_pred):
  """implement MSPE since sklearn does not have it"""
  return 100  * np.mean(np.power((y_true - y_pred) / y_true, 2))

print(f"Mean Squared Error: {mean_squared_error(y_true, y_pred):.3f}")
print(f"Mean Squared Percentage Error: \
      {mean_squared_percentage_error(y_true, y_pred):.3f}")  
```
Output:

```plaintext
Mean Squared Error: 7966.667
Mean Squared Percentage Error: 8.157
```

## Mean Squared Logarithmic Error (MSLE)

MSLE is similar to MSE except that instead of using the squared error, we use the squared logarithmic error.

$$\definecolor{green1}{RGB}{40,162,40} \definecolor{purple1}{RGB}{224,0,224} \definecolor{blue1}{RGB}{116,98,224} \definecolor{orange1}{RGB}{212,106,67} \text{MSLE}\color{blue1}(\color{purple1}y, \color{green1}\hat{y}\color{blue1}) = \color{orange1} \frac{1}{N} \sum_{1}^{N} {\color{blue1}(\log_e(\color{purple1} {y}_{i}+1 \color{blue1}) - \log_e(\color{green1}{\hat y}_{i}+1 \color{blue1}))}^{\color{blue1}2}$$

where

$\color{orange1} N \text{ is the number of samples in your evaluation data set}$,

$\color{purple1} {y}_{i} \text{ is the actual value}$ and

$\color{green1} {\hat y}_{i} \text{ is the predicted value}$.

In practice, we use the square root version of MSLE - the RMSLE just like we use RMSE,  so that the units of the metric and the target values are the same.Note that MLSE penalizes an under-predicted estimate greater than an over-predicted estimate since we use the differences between log values - $\log_e(8) - \log_e(6) \text{is greater than} \log_e{8} - \log_e{10}$. 

```python
print(f"Mean Squared Error: {mean_squared_error(y_true, y_pred):.3f}")
print(f"Mean Squared Log Error: \
      {mean_squared_log_error(y_true, y_pred):.3f}") 
```
Output:

```plaintext
Mean Squared Error: 7966.667
Mean Squared Log Error: 0.050
```

## When to use MAPE, MSPE and (R)MSLE

Of these three metrics, MSLE or its rooted version RMSLE is widely used. It is less biased than the other two metrics for smaller values but it still works with relative errors. scikit-learn's [documentation](https://scikit-learn.org/stable/modules/model_evaluation.html#mean-squared-logarithmic-error) recommends that this metric is best to use when targets have exponential growth, such as population counts, average sales of a commodity over a span of years etc.

MAPE and MSPE can be used when it is more important to penalize errors for smaller values. However note that both of these metrics do not work if one of the target values is a zero - this will cause a division by zero error.