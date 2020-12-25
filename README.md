# Lawrence Berkeley National Lab Numerai Dimensionality Reduction Framework

A data-driven dimensionality reduction analysis utilizing Dask and Scikit-Learn to be used in a paper for Lawrence Berkeley National Laboratory. In here are two notebooks that are works-in-progress that utilize an innovative framework of domain-agnostic dimensionality reduction and feature selection on heavily obfuscated datasets to boost model performance. Positive results in this work could have implications in making public data science competitions feasible with sensitive data with privacy concerns. 

The data comes from two sources: Numer.ai, which is a public finance modeling data science competition, and HVAC/Weather dataset collected by Lawrence Berkeley National Laboratory's weather gauges, which collect several weather indicators such as air temperature, humidity, and dew point. While Numer.ai's dataset is heavily obfuscated with an extremely low signal to noise ratio, the HVAC/Weather dataset is meant to showcase the framework on data which we would consider to be easier to model as we have domain knowledge on the relationships between features (weather) and the target variable (air conditioning/heating usage).

Much of the code only functions in a HPC as the runtimes and core usage are somewhat unrealistic for local machines. However, users who wish to try out this framework may make use of cloud computing environments such as Google Cloud Platform and connect the Dask client to remote clusters in this manner. 

# Dimensionality Reduction Techniques Used

## PCA
Perhaps the most common dimensionality reduction technique used in data science, PCA maximizes the variance between the Principal Components through Singular Value Decomposition. Scikit-learn is used for this technique.

## KPCA
Similar to PCA, however the data is transform with a kernel that is chosen through cross-validation of the model performance. Kernels include linear, polynomial, and radial basis function, as well as others custom kernels. Scikit-learn is used for this technique.

## UMAP
UMAP is an algorithm for dimension reduction based on manifold learning techniques and ideas from topological data analysis. By constructing a nearest neighbor graph with the data the algorithm finds a lower dimensional representation of the data with a similar topological structures of the high dimension nearest neighbor graph. Credit goes to https://umap-learn.readthedocs.io/en/latest/index.html for the package used.

## MDA
Mean Decrease Accuracy is an out-of-bag feature importance algorithm used to rank the overall improvement a feature brings to the model performance when compared to a shuffled version of itself. This version of MDA works with any model type, not just Random Forest, however it is relatively slow in non-parallel code, which is why it is coded using Dask to train the models in parallel, leading to a 500-1000x speed up. 

## SHAP
SHAP (SHapley Additive exPlanations) is a game theoretic approach to explain the output of any machine learning model. It connects optimal credit allocation with local explanations using the classic Shapley values from game theory and their related extensions. Credit goes to https://proceedings.neurips.cc/paper/2017/hash/8a20a8621978632d76c43dfd28b67767-Abstract.html for the package used.

## Feature Clustering
An unique approach to ranking feature importance using MDA by ranking alike feature clusters rather than individual features. We use several innovative distance metrics for feature clustering, such as Variation of Information, Distance correlation, and Maximal Correlation. This way features that are only informative in a collaborative manner are still ranked highly rather than having a low ranking due to being tested on its own.

