#Final Project for CDS303 Fall2026
Anza and Yina

Paper Summary

Understanding purpose of each step in the four step framework

The paper proposes a comprehensive four step framework for explainable cluster analysis to be done on high-dimensional mixed data to address limitations of data quality, explainability, large and mixed datasets. The four steps are: data preprocessing, dimensionality reduction, clustering, and classification.
Data Preprocessing: Real data often has missing variables and outliers and this step addresses it through two functionalities namely: data imputation, and outlier detection. Imputation is a multi-stage method where variables with too much missingness is dropped, and then impute from strongly related variable pairs. While outlier detection is done to identify multivariate outliers. Handling the bad data leads to better clustering results later.
Dimensionality reduction: When the dataset has too many variables it impacts distance-based clustering and is difficult to interpret so data is simplified in this step. Sparse PCA is applied to numerical variables while multiple correspondence analysis is done on categorical variables. 
Clustering: In this step, similar observations are grouped together to find patterns. There is not one universal clustering approach that works best and it depends on the field of study in question. 
Classification: After clustering, each cluster has a cluster label. The classification model helps to explain the clusters and is used to predict those labels from the original variables. 

Why clustering needs interpretation

Clustering falls under unsupervised learning meaning there are no set answers to check the clustered groups against, hence requiring interpretation. Clustering results return arbitrary labels which are difficult to interpret alone. Validity metric also only show that clusters are compact and separated not if they are meaningful or not. The clustered results are shaped by data preprocessing and dimensionality reduction so the results need to be traced to original ones to be well interpreted. 

How classification helps explain clusters

Classification helps explain clusters by turning cluster labels into a target variable and using the original variables as predictors. Then a supervised model is used to learn which variables determine cluster membership and SHAP values show which variables matter most across all clusters and within each one. It also helps to identify whether high or low values push an observation toward a particular cluster. In the paper’s case study, this helped in turning arbitrary labels into named groups such as “Disadvantaged students” by revealing most important variables for each cluster.



