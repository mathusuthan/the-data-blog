---
title: Metrics for Classification - Part 1
date: "2020-03-05T00:00:00"
lastMod: "2020-03-05T00:00:00"
math: true
draft: false
diagram: false
tags: ["Machine Learning", "Classification"]
keywords: ["Machine Learning", "Classification", ]
image: 
  placement: 3
  caption: "Photo by Joshua Eckstein on Unsplash"

---

Classification Models try to identify the correct class or category of the target. There are many metrics available - we will look at the most common ones: Accuracy, Precision, Recall first and then explore the others in Part 2. Similarly let us focus on binary classification problems first before discussing multi-class classification.

## The Confusion Matrix

A confusion matrix is a technique for summarizing the performance of a classification model. 

{{< figure src="confusion_matrix_1.png" title="Confusion Matrix" lightbox="true" >}}

As the [Wikipedia definition](https://en.wikipedia.org/wiki/Confusion_matrix) describes it, it is called a confusion matrix because it gives you an idea if the model is confusing one class for the other i.e., mislabelling one as another.

From the figure above, using Class A as our positive (desired) class and Class B as negative class, we can define :

- **True Positives** : Members of Class A are correctly classified as Class A.
- **True Negatives** : Members of Class B are correctly classified as Class B.
- **False Positives** : Members of Class B are incorrectly classified as Class A.
- **False Negatives** : Members of Class A are incorrectly classified as Class B.

Using the confusion matrix, it is easy to derive the other metrics from it. Let us start looking at them below.

## Accuracy

$$ Accuracy = \frac {\color{green}{TP} + \color{green}{TN}} {\color{green}{TP} + \color{red}{FP} + \color{green}{TN} + \color{red}{FN}} $$

Accuracy gives us the idea of the overall performance of the model. It tells us what fraction of predictions are correct, both for the positive and negative classes. It is easier to remember Accuracy as follows:

$$ Accuracy = \frac {\text{Number of Correct Predictions}} {\text{Total Number of Predictions}} $$

For example, let's try calculating accuracy for the following model that classified 100 tumors as malignant (the positive class) or benign (the negative class)[^1]:

<table style="background-color: inherit">
  <tbody><tr >
    <td>
    <font color="#28a745">
      <b>True Positive (TP):</b>
      <ul>
        <li>Reality: Malignant</li>
        <li>ML model predicted: Malignant</li>
        <li><strong>Number of TP results: 1</strong></li>
      </ul></font>
    </td>
    <td >
    <font color="#dc3545">
      <b>False Positive (FP):</b>
      <ul>
        <li>Reality: Benign</li>
        <li>ML model predicted: Malignant</li>
        <li><strong>Number of FP results: 1</strong></li>
    </ul></font></td>
  </tr>
  <tr>
    <td >
    <font color="#dc3545">
      <b>False Negative (FN):</b>
      <ul>
        <li>Reality: Malignant</li>
        <li>ML model predicted: Benign</li>
        <li><strong>Number of FN results: 8</strong></li>
      </ul></font>
    </td>
    <td>
    <font color="#28a745">
      <b>True Negative (TN):</b>
      <ul>
        <li>Reality: Benign</li>
        <li>ML model predicted: Benign</li>
        <li><strong>Number of TN results: 90</strong></li>
      </ul></font>
    </td>
  </tr>
</tbody></table>

Accuracy = $ \frac {\color{green}{TP} + \color{green}{TN}} {\color{green}{TP} + \color{red}{FP} + \color{green}{TN} + \color{red}{FN}} $ = $ \frac {\color{green}{1} + \color{green}{90}} {\color{green}{1} + \color{red}{1} + \color{green}{90} + \color{red}{8}}  $ = $ 0.91 $

But Accuracy as a metric has a major disadvantage when our data set has imbalanced classes (lots of samples of one class). A very familiar example is credit card transactions - a majority of the transactions are genuine but we are more interested in predicting the fraudulent transactions. In this case, our accuracy for the model will be high even if we predict all the transactions are genuine. In cases where the data is imbalanced, we need to use other metrics like Precision and Recall.

{{% alert warning %}}
Do not use Accuracy when your data is very imbalanced. Precision or Recall are better metrics in those cases.
{{% /alert %}}

## Precision

$$ Precision = \frac {\color{green}{TP}} {\color{green}{TP} + \color{red}{FP} } $$

{{% alert hint %}}
Precision tells us the proportion of the positive predictions that were correct.
{{% /alert %}}

Let us calculate precision for the tumor detection model from the above section:

<table>
  <tbody><tr>
    <td width="50%">
    <font color="#28a745">
      <b>True Positives (TPs): 1</b>
      </font>
    </td>
    <td >
    <font color="#dc3545">
      <b>False Positives (FPs): 1</b>
      </font>
    </td>
  </tr>
  <tr>
    <td >
    <font color="#dc3545">
      <b>False Negatives (FNs): 8</b>
    </font>
    </td>
    <td >
    <font color="#28a745">
      <b>True Negatives (TNs): 90</b>
    </font>
    </td>
  </tr>
</tbody></table>

Precision = $ \frac {\color{green}{TP}} {\color{green}{TP} + \color{red}{FP} } $ = $ \frac {\color{green}{1}} {\color{green}{1} + \color{red}{1} } $ = $ 0.5 $

The precision value of 0.5 tells us that the model is correct 50% of the time when it predicts a tumor is malignant.

## Recall

$$ Recall = \frac {\color{green}{TP}} {\color{green}{TP}  + \color{red}{FN}} $$

{{% alert hint %}}
Recall tells us the proportion of actual positives that were correctly predicted.
{{% /alert %}}

Let us calculate recall for the tumor detection model :

<table>
  <tbody><tr>
    <td width="50%">
    <font color="#28a745">
      <b>True Positives (TPs): 1</b>
      </font>
    </td>
    <td >
    <font color="#dc3545">
      <b>False Positives (FPs): 1</b>
      </font>
    </td>
  </tr>
  <tr>
    <td >
    <font color="#dc3545">
      <b>False Negatives (FNs): 8</b>
    </font>
    </td>
    <td >
    <font color="#28a745">
      <b>True Negatives (TNs): 90</b>
    </font>
    </td>
  </tr>
</tbody></table>

Recall = $ \frac {\color{green}{TP}} {\color{green}{TP} + \color{red}{FN} } $ = $ \frac {\color{green}{1}} {\color{green}{1} + \color{red}{8} } $ = $ 0.11 $

The recall value of 0.11 tells us that the model identifies malignant tumors correctly 11% of the time.

## The Precision / Recall Tradeoffs and other metrics

Precision and Recall are inversely related. You cannot increase Precision without decreasing Recall and vice versa. Part 2 of this post will discuss this tradeoff along with metrics that use both Recall and Precision (F-scores), and other metrics widely used for Classification models.
[^1]: [From the Google Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/classification/accuracy)