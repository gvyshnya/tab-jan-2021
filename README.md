# Introduction
This repo contains the Jupyter notebooks with various AutoML and ML Experiments on Kaggle's Jan 2021 Tabular Contest Dataset (https://www.kaggle.com/c/tabular-playground-series-jan-2021)

# Feature Importance Experiments

A number of Feature Importance experiments have been conducted during the feature engineering process, using *featurewiz* library.

A number of simple statistical and genetic features (like sums, diffs, products of the original numeric variables etc.) has been tried and scrutinized.

It appeared that none of such new features above added the edge in the ML model performance. It was true for the models below
- various manually tuned *lightgbm* and *xgboost* models 
- 3 AutoML-backed models, using *AutoViML*, *Auto Keras* and *H2O AutoML*

# AutoML Product Comparison on the Dataset

Out of the three tools above, *H2O AutoML* appeared to demonstrate the best performance (on the set of the basic raw features). However, it required quite a lot of time (8+ h) to get it up to the level to  perform so.

The rest of AutoML tools performed worse in this experiment (they did not beat the manually tuned *lightgbm* and *xgboost* models).

However, *AutoViML* was really impressive as it was able to train quite a good model in less then 10 mins.

# Troubleshooting

When you use AutoML tools, you should be ready for some upfront configuration/setup tweaks on your local machine before you are able to use it effectively.

In the sections below, I am going to share the takeaways from the problem resolutions during AutoViML and Auto Keras setup under Windows 10/Anaconda and Linux Subsystem for Windows 10 platforms.

