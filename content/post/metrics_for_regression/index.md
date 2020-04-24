---
title: Metrics for Regression - Part 1
date: "2020-02-03T00:00:00"
lastMod: "2020-03-07T00:00:00"
draft: false
math: true
diagram: false
tags: ["Machine Learning", "Regression"]
keywords: ["Machine Learning", "Regression"]
image: 
  placement: 3 
  caption: 'Photo by Ryan Stone on Unsplash'
---

Regression models try to predict a quantitative variable. The most widely used metrics for evaluating the accuracy of regression models are the  Mean Absolute Error (MAE), Mean Squared Error (MSE) and the Median Absolute Error (MedAE).

## Mean Absolute Error (MAE)

MAE measures the absolute distance of the predictions from the target values.  The mathematical formula is given below:

$$\definecolor{green1}{RGB}{40,162,40} \definecolor{purple1}{RGB}{224,0,224} \definecolor{blue1}{RGB}{116,98,224} \definecolor{orange1}{RGB}{212,106,67} \text{MAE}\color{blue1}(\color{purple1}y, \color{green1}\hat{y}\color{blue1}) = \color{orange1} \frac{1}{N} \sum_{1}^{N} {\color{blue1}\left|\color{purple1} {y}_{i} -\color{green1}{\hat y}_{i} \right|}$$

where

$\color{orange1} N \text{ is the number of samples in your evaluation data set}$,

$\color{purple1} {y}_{i} \text{ is the actual value}$ and

$\color{green1} {\hat y}_{i} \text{ is the predicted value}$.

## Mean Squared Error (MSE)

MSE measures the squared distance of the predictions from the target values.  The mathematical formula is given below:

$$\definecolor{green1}{RGB}{40,162,40} \definecolor{purple1}{RGB}{224,0,224} \definecolor{blue1}{RGB}{116,98,224} \definecolor{orange1}{RGB}{212,106,67} \text{MSE}\color{blue1}(\color{purple1}y, \color{green1}\hat{y}\color{blue1}) = \color{orange1} \frac{1}{N} \sum_{1}^{N} {\color{blue1}(\color{purple1} {y}_{i} -\color{green1}{\hat y}_{i} \color{blue1})}^{\color{blue1}2}$$

where

$\color{orange1} N \text{ is the number of samples in your evaluation data set}$,

$\color{purple1} {y}_{i} \text{ is the actual value}$ and

$\color{green1} {\hat y}_{i} \text{ is the predicted value}$.

For easier interpretation of MSE with respect to the target values, we usually take the root of the MSE - this is known as the Root Mean Squared Error (RMSE).

$$RMSE = \sqrt{MSE}$$

## Median Absolute Error (MedAE)

MedAE is given by the median of all the absolute distance of the predictions from the target values.  The mathematical formula is given below:

$$\definecolor{green1}{RGB}{40,162,40} \definecolor{purple1}{RGB}{224,0,224} \definecolor{blue1}{RGB}{116,98,224} \definecolor{orange1}{RGB}{212,106,67}\text{MedAE} \color{blue1}(\color{purple1}y, \color{green1}\hat{y} \color{blue1}) = \color{orange1}\text{median} \color{blue1}(\mid \color{purple1}y_1 - \color{green1}\hat{y}_1 \mid, \ldots, \mid \color{purple1}y_n - \color{green1}\hat{y}_n \mid \color{blue1})$$


where

$\color{orange1} median \text{ is the median function}$,

$\color{purple1} {y}_{1..n} \text{ are the actual values}$ and

$\color{green1} {\hat y}_{1..n} \text{ are the predicted values}$.

## How to use MAE, MSE and MedAE

When we evaluate our regression model, we want the error to be as low as possible. So ideally, we would like the MSE, MAE, MedAE values to be low.

One thumb of rule to remember is that we need to do better than the mean of the target values for MSE and the median of the target values for MAE.

## When to use MAE vs., MSE and MedAE

Use MAE when you have outliers in your target variable. Otherwise use MSE. The reason for this is MAE corresponds to the $l1$-norm loss whereas MSE corresponds to the $l2$-norm loss. Higher the norm, higher the influence of larger values on the metric. For example, let us look at the following calculation of MAE and RMSE in `scikit-learn`.

```python
# Without outliers

import numpy as np
from sklearn.metrics import mean_absolute_error
from sklearn.metrics import mean_squared_error

y_true = [-0.5, 2, 3, 5, 7]
y_pred = [0.0, 2, 2.5, 4, 8]

mae = mean_absolute_error(y_true, y_pred)
rmse = np.sqrt(mean_squared_error(y_true, y_pred))

print(f"MAE is {mae:.3f} while RMSE is {rmse:.3f}")
```

The output is:

```plaintext
MAE is 0.600 while RMSE is 0.707
```

Let us change the last value in `y_true` from 7 to 15 without changing the predictions in `y_pred` and observe the changes in the values of MAE and RMSE.

```python
# With one outlier

import numpy as np
from sklearn.metrics import mean_absolute_error
from sklearn.metrics import mean_squared_error

y_true = [-0.5, 2, 3, 5, 20]
y_pred = [0.0, 2, 2.5, 4, 8]

mae = mean_absolute_error(y_true, y_pred)
rmse = np.sqrt(mean_squared_error(y_true, y_pred))

print(f"MAE is {mae:.3f} while RMSE is {rmse:.3f}")
```

Now the values for MAE and RMSE are:

```plaintext
MAE is 1.800 while RMSE is 3.178
```

While both the metrics where affected by the presence of the outlier, the impact of just one outlier on RMSE is greater than that for MAE. So, when we are absolutely sure that we have outliers in the target, using MAE gives us a better idea of how our model fits to the data.

If we have target values that are extreme or unusual but not outliers, we can use RMSE. The idea behind that is if the target values are following a normal distribution, we can still expect some of the values from the tail to be very high or low (so called _black swans_, extremely unlikely but still probable).

And finally, MedAE is also used in cases where we have outliers in data.In scikit-learn, it is available as `sklearn.metrics.median_absolute_error`.

In a [subsequent post](../more_metrics_for_regression), we will discuss other metrics used for Regression.
