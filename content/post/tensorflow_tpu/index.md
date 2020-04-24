---
title: "Running Keras and TensorFlow 2 models on a TPU"
date: "2020-02-12T00:00:00"
lastMod: "2020-02-12T00:00:00"
draft: true
math: true
diagram: false
tags: ["Deep Learning", "TensorFlow", "Keras", "TPU"]
keywords: ["Deep Learning", "TensorFlow", "Keras", "TPU"]
image: 
  placement: 3 
  caption: 'TPU v3'
---

Recently Kaggle announced that they will now support Tensor Processing Units (TPU) in the Kernels. Along with the announcement, a [competition](https://www.kaggle.com/c/flower-classification-with-tpus/overview) to classify over 100 species of flowers was launched as well. While TPUs are available in Google Cloud for rent and in Google Colab for research, including them in Kaggle Kernels should mean faster and more experiments.

In this post, I will discuss briefly on what a TPU is and how we can code for TPU in TensorFlow 2 and Keras.

## Why TPUs ?
While it is usual to execute the Deep Learning models in a GPU _(or a GPU cluster)_, GPUs were not invented for Deep Learning. They are general purpose parallel programmable cores and support tasks like computer graphics, simulations, and of course Deep Learning. TPUs were purpose built for Deep Learning (or to be specific for any task where large matrix multiplications dominate like Neural Networks). For more information on the architecture of TPUs, I recommend this [document](https://cloud.google.com/tpu/docs/system-architecture#hardware_architecture) from Google Cloud.

## Using TPUs in Keras

A generic way to detect the TPU and if present, execute it is:

```python
# TensorFlow 2.1
import tensorflow as tf

# TPU detection  
try:
  tpu = tf.distribute.cluster_resolver.TPUClusterResolver()
except ValueError:
  tpu = None

# TPUStrategy for distributed training
if tpu:
  tf.config.experimental_connect_to_cluster(tpu)
  tf.tpu.experimental.initialize_tpu_system(tpu)
  strategy = tf.distribute.experimental.TPUStrategy(tpu)
else: # default strategy that works on CPU and single GPU
  strategy = tf.distribute.get_strategy()

# use TPUStrategy scope to define model
with strategy.scope():
  model = tf.keras.Sequential( ... )
  model. compile( ... )

# train model normally on a tf.data.Dataset
model.fit(training_dataset, epochs=EPOCHS, steps_per_epoch=...)
```