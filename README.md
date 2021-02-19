# Introduction
This repo contains the Jupyter notebooks with various AutoML and ML Experiments on Kaggle's Jan 2021 Tabular Contest Dataset (https://www.kaggle.com/c/tabular-playground-series-jan-2021)

# Feature Importance Experiments

A number of Feature Importance experiments have been conducted during the feature engineering process, using *featurewiz* library.

A number of simple statistical and genetic features (like sums, diffs, products of the original numeric variables etc.) has been tried and scrutinized.

It appeared that none of such new features above added the edge in the ML model performance. It was true for the models below
- various manually tuned *lightgbm* and *xgboost* models 
- 3 AutoML-backed models, using *AutoViML*, *AutoKeras* and *H2O AutoML*

Note: Running *FeatureWiz Feature Selection - Genetic Dividers.ipynb* notebook under Windows 10 (Python 3.7, Anaconda) resulted in crashing both Anaconda runtime and the browser where Jupyter notebooks were operating.

# AutoML Product Comparison on the Dataset

Out of the three tools above, *H2O AutoML* appeared to demonstrate the best performance (on the set of the basic raw features). However, it required quite a lot of time (8+ h) to get it up to the level to  perform so.

The rest of AutoML tools performed worse in this experiment (they did not beat the manually tuned *lightgbm* and *xgboost* models).

However, *AutoViML* was really impressive as it was able to train quite a good model in less then 10 mins.

# Troubleshooting

When you use AutoML tools, you should be ready for some upfront configuration/setup tweaks on your local machine before you are able to use it effectively.

In the sections below, I am going to share the takeaways from the problem resolutions during AutoViML and Auto Keras setup under Windows 10/Anaconda and Linux Subsystem for Windows 10 platforms.

## AutoViML Installation and Configuration

### Resolving cmake dependencies

*AutoViML* internally uses *xgboost* to do its  machinery, and therefore you have to install it to your system first.

When I tried to do it in my Linux Subsystem for Windows 10 environment, the  installation of *xgboost* kept failing due to the  lack of *cmake* tools.

For my target environment, there is no any binary installation of *cmake*, and the only option was to compile it from the source code.

Below are the steps taken to accomplish it

1/ Updating the system by running the commands below

`sudo apt update`

`sudo apt upgrade`

2/ Download the sources of the latest version of cmake - that is, 3.19.4 as of the moment

`sudo wget https://github.com/Kitware/CMake/releases/download/v3.19.4/cmake-3.19.4.tar.gz`

3/ Unpack the source code archive locally

`sudo tar -zxvf cmake-3.19.4.tar.gz`

4/ Make its bootstrap executable

`cd cmake-3.19.4`

`sudo chmod 755 bootstrap`

5/ Complete the compilation of *cmake*

`sudo ./bootsrap`

`sudo make`

6/ Install the compiled binaries of *cmake* in your system

`sudo make install`

7/ Just in case, you check the version of *cmake*

`cmake --version`

After it, the process to install *xgboost* was successful.

*Note*: additional details on the installation of *cmake* from sources explained in https://www.fosslinux.com/38392/how-to-install-cmake-on-ubuntu.htm

### Resolving llvmlite dependencies

One of the packages that *AutoViML* required the operable instance of *llvm*

The resolution to this issue was similar to what explained in https://github.com/slundberg/shap/issues/1508 implemented

1/ Install llvm

`sudo apt-get install llvm`

2/ Upgrade llvm to v. 10 (the command above would bring v.6.0.0)

`sudo apt-get install llvm-10*`

3/ Change the llvm config to point to v.10, as explained in https://askubuntu.com/questions/1286131/how-do-i-install-llvm-10-on-ubuntu-18-04

4/ Install the python package for llvmlite

`pip install llvmlite`

After completing the steps above, it was possible to install *AutoViML* itself successfully.

## AutoKeras Installation and Configuration

When installing AutoKeras into my Windows 10 environment, I faced the issue below

```
ImportError: Traceback (most recent call last):
  File "C:\ProgramData\Anaconda3\lib\site-packages\tensorflow\python\pywrap_tensorflow.py", line 64, in <module>
    from tensorflow.python._pywrap_tensorflow_internal import *
ImportError: DLL load failed: The specified module could not be found.
```

The root cause/resolution have been explained in  

- https://stackoverflow.com/questions/44503603/tensorflow-on-windows-importerror-dll-load-failed-the-specified-module-could
- https://support.microsoft.com/en-us/topic/the-latest-supported-visual-c-downloads-2647da03-1eea-4433-9aff-95f26a218cc0

So upgrading the Visual C++ runtime components to the latest version helped to fix the issue.