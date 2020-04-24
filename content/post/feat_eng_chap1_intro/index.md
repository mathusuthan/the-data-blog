---
title: "Notes from Feature Engineering and Selection - Chapter 1: Introduction"
date: "2020-02-11T00:00:00"
lastMod: "2020-02-11T00:00:00"
draft: false
math: false
diagram: false
tags: ["Feature Engineering", "Notes", "Books", "FES"]
keywords: ["Feature Engineering", "Notes", "Books"]
image: 
  placement: 3 
  caption: ''
---

{{< figure src="cover.jpg" lightbox="true" >}} I have been going through the book "[Feature Engineering and Selection: A Practical Approach for Predictive Models](http://feat.engineering)" by Max Kuhn and Kjell Johnson. I am a big fan of their previous book "[Applied Predictive Modeling](http://appliedpredictivemodeling.com/)"  and thought it might be useful to publish my notes from the new book as a reference for my future self and for any other readers who might be interested. And of course, the book is available for [free online](http://feat.engineering) if anybody wants to check out the whole book.


## Introduction

- Models are created from existing data and finding a mathematical representation that faithfully approximates the data.
- Models can be:
  - **Inferential**: a variable of interest is compared to the estimated uncertainty in the data and a determination is made based on how uncommon such a result is relative to the noise (for e.g., using statistical hypothesis tests). 
  - **Predictive**: an estimation problem where the goal is to estimate a particular value as accurately as possible. For example, predicting arrival times for flights.

- Important characteristics of a model:
  - **Accuracy**: How accurate are the predictions? What is the uncertainty in the prediction? 
  - **Simplicity**: A simple model is generally preferable to a complex one, especially for inference. However there is no point in having a simple model if its accuracy is very bad. A complex model usually increases accuracy but might be much less interpretable.
  - **Robustness**: How well does a model predict for data with extreme or missing values?

- **Feature Engineering**: There are different ways to represent predictors for different models in order to get the best results out of them - this is the idea behind feature engineering: the process of creating representations of data that increase the effectiveness of a model.

- **Models** : No model will be accurate if the data is irrelevant (for e.g., the predictors have no relationship to the outcome). However there are many models each with its own sensitivities and needs. For e.g.,
  - Some models cannot tolerate predictors that are multicollinear (for e.g., linear models).
  - Many models cannot use samples with any missing values.
  - Some models are severely compromised when irrelevant predictors are present in the data.

- To achieve the characteristics of a good model or to at the least have a good trade-off between them, it is important to understand the interplay between predictors used in the model  and the type of model. Accuracy and/or simplicity can sometimes be improved by representing data in ways appropriate to the model or by reducing the number of variables used.

## Important Concepts

- **Overfitting**: Overfitting refers to a situation where the predictive model fits very well to the current data but fails when predicting new samples (_test data_).

- Models that are very flexible (tree based models, neural nets) tend to overfit. They need regularization to avoid overfitting.

- While models can overfit to the _data_ points, feature selection techniques can overfit to the _predictors_. This occurs when during training, a variable appears to have some influence on the outcome but shows no real relationship with the outcome on new data. This usually occurs when the number of data points in the sample (n) is small and the number of predictors (p) is large.

- **Supervised and Unsupervised Analysis**: Supervised data analysis involves identifying patterns between predictors and an identified outcome. While unsupervised data analysis focusses on finding structure withing the data. While both these methods are prone to overfitting, supervised analyses are particularly inclined to error due to the {{< hl >}}_self-fulfilling predictive prophecy_{{< /hl >}}: the analyst tends to focus on the significant relationships in the current data at hand without analyzing if they would hold true on unseen data too.

- **"No Free Lunch" Theorem**: Without any specific knowledge of the problem or data at hand, no one predictive model can be said to be the best. Therefore, in practice, it is better to try a number of different types of models to check which works best for your particular data set.

- **The Modeling Process**: The modeling process is an iterative and heuristic one. It is difficult to anticipate the requirements to model a data set efficiently and therefor it is common for many approaches in variable preprocessing, transformations, imputation, hyperparameter tuning, model selection to be evaluated and verified before settling on one.
{{< figure src="modeling-process.png" title="Typical Modeling Process" lightbox="true" >}}

- **Variance and Bias**: Variance describes the degree in which the values of a variable can fluctuate. There can be a high fluctuation (_high variance_) as well as a low fluctuation (_low variance_). Bias is the degree in which something deviates from its true underlying structure or value. There can be a major deviation (_high bias_) or a minor one (_low bias_).

- **Model Variance**: A model has high variance if small changes to the underlying data used to estimate the parameters cause a sizable change in those parameters (or in the structure of the model).
  - For e.g., the sample mean of a set of data points has higher variance than the sample median.This is because the sample median uses only the values in the center of the data distribution. Therefore it is insensitive to moderate changes in the values.

  - **Low-Variance Models**: they use all of the data to estimate the model parameters. Examples of low variance models are Linear Regression, Logistic Regression and Partial Least Squares.

  - **High-Variance Models**: they rely on individual data points to define their parameters. Examples are Classification or Regression trees, Nearest Neighbor models and Neural Networks.

- **Model Bias**: Model Bias reflects the ability of a model to conform to the underlying theoretical structure of the data.
  - **Low-Bias Models**: A low-bias model is one that is highly flexible and has the capacity to fit a variety of different shapes and patterns. Examples are Tree-based models, Support Vector Machines and Neural Networks.

  - **High-Bias Models**: A high-bias model would be unable to estimate values close to their true theoretical counterparts. For example, Linear Methods often have a very high bias since they cannot describe nonlinear patterns in the predictor variables.

- **The Bias-Variance tradeoff**:  model bias and variance can often be in opposition to one another; in order to achieve low bias, models tend to demonstrate high variance (and vice versa).

- It is desirable to have a low-bias, low-variance model. One method of creating a low-bias, low-variance model is to augment a low-variance model with appropriate representations of the data to decrease the bias.

- **Big Data**: 
  - Big Data does not necessarily mean better data.
    - The data may not be diverse. They might simply be numerous examples of a single class of data.
    - Big data cannot automatically induce a relationship between the predictors and outcome when none actually exists.
  - Not all models can work with Big Data. And models that work with Big Data are computationally expensive.
  - Domains with unstructured / unlabelled data can benefit from having Big Data.

- **Things to remember when building predictive models**:
  - When modeling data, there is almost never a single model fit or feature set that will immediately solve the problem. The process is more likely to be a campaign of trial and error to achieve the best results.
  - The effect of feature sets can be much larger than the effect of different models.
  - The interplay between models and features is complex and somewhat unpredictable.
  - With the right set of predictors, is it common that many different types of models can achieve the same level of performance.
