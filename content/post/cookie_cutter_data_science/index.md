---
title: "How to organize a Data Science project"
date: "2020-04-28T00:00:00"
lastMod: "2020-04-28T00:00:00"
draft: false
math: false
diagram: false
tags: ["Data Science", "Tools", "Machine Learning", "Reproducible Research"]
keywords: ["Data Science", "Tools", "Machine Learning", "Reproducible Research"]
image: 
  placement: 3 
  caption: 'Photo by Jason Jarrach on Unsplash'
---

With the proliferation of tools like Jupyter and Colab, it is easy to do a complete Data Science or ML project on a single 'notebook'. Typically, I have seen people doing the entire development on them and then saving the model as a `pickle` file (usually , sklearn models)  or a `HDF5` (TensorFlow models). These serialized models are then used for serving in production. 

The problem with this approach is that there is a sudden explosion of Jupyter notebooks and  serialized models with different versions. It can become a nightmare if someone else were to try and replicate a particular model or to enhance it.

One simple solution is to ensure everyone uses a specific template to organize their code and models. Each new model could be then stored as a different release in the organization's version control repository along with all the source code. I recommend using the [Cookiecutter Data Science](https://drivendata.github.io/cookiecutter-data-science/) structure to organize the code. The default directory structure proposed by the tool is as below: 

```
├── LICENSE
├── Makefile           <- Makefile with commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── src                <- Source code for use in this project.
│   ├── __init__.py    <- Makes src a Python module
│   │
│   ├── data           <- Scripts to download or generate data
│   │   └── make_dataset.py
│   │
│   ├── features       <- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         <- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  <- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
│
└── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io
```

You can always customize the directory structure as per your organization's needs but the idea is that everyone in your team follows the same structure. 

It is easy to get started with this template:
1. From your shell, install `cookiecutter` Python package with pip

  ```
  pip install cookiecutter
  ```
or conda 

  ```
  conda config --add channels conda-forge
  conda install cookiecutter
  ```
2. To start a new project run:

  ```
  cookiecutter https://github.com/drivendata/cookiecutter-data-science
  ```
You will be prompted for information on your project name, git repo name and S3 bucket (optional). Enter them and viola, a new project is created for you.

If you want to customize the project structure, you can clone the Cookiecutter Data Science [repo](https://github.com/drivendata/cookiecutter-data-science) and make the necessary changes to it. After that, you can use the command
  ```
  cookiecutter <your-project-template> 
  ```
to create a project based on your template.

## But what if i use R

I generally use the cookiecutter template with a few modifications for both my R and Python projects. But if you only looking for a way to organize your R code, I recommend using the library [ProjectTemplate](http://projecttemplate.net/).

Happy coding !!