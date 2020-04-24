---
title: Metrics for Classification - Part 2
date: "2020-03-11T00:00:00"
lastMod: "2020-03-11T00:00:00"
math: true
draft: false
diagram: false
tags: ["Machine Learning", "Classification"]
keywords: ["Machine Learning", "Classification", ]
image: 
  placement: 3
  caption: "Photo by Faris Mohammed on Unsplash"

---

A [previous post](../metrics_for_classification) discussed the Accuracy, Precision and Recall metrics for Classification models. This post will discuss F-scores, AUC and how to choose the appropriate metric.

## F1 Score

The F1 Score (a.k.a balanced F-score or F-measure) is a [harmonic mean](https://en.wikipedia.org/wiki/Harmonic_mean) of Precision and Recall. It provides a balanced measure of the performance of the model, scoring high if both Precision and Recall are high, or low otherwise.

$$ F1 = 2 \cdot \frac {precision \cdot recall}{precision + recall} $$

Let us look at F1 Score in scikit-learn.

```python
from sklearn.metrics import f1_score

y_true = [0, 1, 0, 0, 1, 1]
y_pred = [0, 0, 1, 0, 0, 1]

f1_score(y_true, y_pred)
```

```plaintext
0.4
```

## ROC AUC

A Receiver operating characteristic (ROC) graph plots the True Positive Rate (TPR), another name for Recall, against the False Positive Rate (FPR) for different decision thresholds of the classifier. The area under this ROC curve (AUC) is then used to compare the performance of different classification models.

{{< figure src="roc1.png" title="" lightbox="true" >}}

The above graph shows the area for 
- an ideal classifier ($\color{green}\text{green color curve}$) which has a true positive rate of 1 : it has classified all the items correctly. 
- a random guess classifier that has classified 50% of the items accurately ($\color{orange}\text{orange color curve}$) and lies in the diagonal. Any curve below this line indicates a badly performing classifier that is worse even than random guessing.

Usually the curve for classification models will lie somewhere in between the green and the blue curves. We can compare different classifiers using the ROC AUC score. scikit-learn provides methods to both [plot a ROC curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html#sklearn.metrics.roc_curve) and [calculate the ROC AUC]([https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) score.

## Which metric to use and when

Let us consider different scenarios and  decide which metric to use in those contexts.

1. __Disease Detection__: Imagine we have an infectious disease spreading and we want to evacuate only people *without disease* from the city. We have been given a choice of three different testing vendors and have to evaluate their equipment based on actual results from the field. In this case which metric we should consider? <br>
In this hypothetical scenario, our positive class is people without the disease and our negative class is people with the disease. And we want to be very sure that we do not allow people with the disease to be evacuated, since then they could be spreading the disease to the healthy people - that is we do not want false positives.<br>
In this case, precision would be the best metric to use since it penalizes any false positives (_people with disease categorized as healthy in our scenario_). Recall that $$ Precision = \frac {\color{green}\text{True Positives}} {\color{green}\text{True Positives} + \color{red}\text{False Positives} } $$

2. __Fraud Detection__: Imagine we are building a model for a credit card company to detect fraudulent transaction. We want to be sure our model detects almost all of the fraudulent transactions so we can stop them, and we are okay with a few of the regular transactions (_false positives_) being stopped too.<br>
In this scenario we want a model with very high recall since we want to penalize false negatives more than false positives. Just to remind ourselves, 
$$ Recall = \frac {\color{green}\text{True Positives}} {\color{green}\text{True Positives} + \color{red}\text{False Negatives} } $$

3. Accuracy metric can be used when the dataset is not skewed.<br>
F-Scores can be used when we value both precision and recall equally.

We will discuss metrics for multi-label and multi-class classification problems in a future article.