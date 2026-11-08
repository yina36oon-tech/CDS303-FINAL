# Section 1
## Paper Summary
### Understanding purpose of each step in the four step framework

The paper proposes a comprehensive four step framework for explainable cluster analysis to be done on high-dimensional mixed data to address limitations of data quality, explainability, large and mixed datasets. The four steps are: data preprocessing, dimensionality reduction, clustering, and classification.
Data Preprocessing: Real data often has missing variables and outliers and this step addresses it through two functionalities namely: data imputation, and outlier detection. Imputation is a multi-stage method where variables with too much missingness is dropped, and then impute from strongly related variable pairs. While outlier detection is done to identify multivariate outliers. Handling the bad data leads to better clustering results later.
Dimensionality reduction: When the dataset has too many variables it impacts distance-based clustering and is difficult to interpret so data is simplified in this step. Sparse PCA is applied to numerical variables while multiple correspondence analysis is done on categorical variables. 
Clustering: In this step, similar observations are grouped together to find patterns. There is not one universal clustering approach that works best and it depends on the field of study in question. 
Classification: After clustering, each cluster has a cluster label. The classification model helps to explain the clusters and is used to predict those labels from the original variables. 

### Why clustering needs interpretation

Clustering falls under unsupervised learning meaning there are no set answers to check the clustered groups against, hence requiring interpretation. Clustering results return arbitrary labels which are difficult to interpret alone. Validity metric also only show that clusters are compact and separated not if they are meaningful or not. The clustered results are shaped by data preprocessing and dimensionality reduction so the results need to be traced to original ones to be well interpreted. 

### How classification helps explain clusters

Classification helps explain clusters by turning cluster labels into a target variable and using the original variables as predictors. Then a supervised model is used to learn which variables determine cluster membership and SHAP values show which variables matter most across all clusters and within each one. It also helps to identify whether high or low values push an observation toward a particular cluster. In the paper’s case study, this helped in turning arbitrary labels into named groups such as “Disadvantaged students” by revealing most important variables for each cluster.

## Description of current data and source

Current Dataset: Washington DC Crime Incidents in 2025 

Source: DC Open Data, ‘Crime Incidents in 2025’, published by the Metropolitan Police Department of Washington, DC.

The dataset contains 24,156 records of reported crime incidents in Washington, DC. The reports span the time frame from January 1, 2025 to January 1, 2026. There are 25 different variables that describe each incident. The key variables included and focused in our project are as follows:

- The Latitude and Longitude which help to pinpoint the location the incident occurred 

- Report Date refers to when the incident was reported 

- Block gives the block-level street address showing location roughly different from previous location variables

- Offense variable describes what kind of crime took place and Method variable indicates how the crime was committed

Other variables include Start Date, End Date of the incident, Block X and Block Y coordinates explain location further while Ward, District, and Police Service Area help to describe the administrative and police jurisdictions under which the incident occurred. Voting Precinct provides further geographic data on the incident and Census tract are statistical subdivisions that can be used to link crimes to census and demographic data 

## Variable Selection Table

| Variable | Description and unit | Data type | Proposed role | Rationale / preparation needed |
|---|---|---|---|---|
| LATITUDE | North-south location of the incident | Numerical | Clustering input | Indicates location of crime. Needs standardization before clustering. |
| LONGITUDE | East-west location of the incident | Numerical | Clustering input | Indicates location of crime. Needs standardization before clustering. |
| REPORT_DAT | Date and time the crime was reported (UTC timezone) | Object as datetime | Clustering input | Indicates time and date of crime. Need to convert from UTC to New_York timezone, then get hour and day of week from the entire attribute. |
| OFFENSE | 9 types of crime | Categorical | Classification input | Explains what kind of crime each cluster contains. Needs to group rarer/more severe cases|
| METHOD | How the offense was committed (weapon, etc.) | Categorical | Classification input | Adds more detail to explain clusters|
| X, Y, XBLOCK, YBLOCK | Map coordinates | Numerical | Exclude | Redundant with LATITUTE and LONGITUDE|
| OBJECTID, OCTO_RECORD_ID | Record identifiers | ID | Exclude | Not meaningful for analysis |
| START_DATE, END_DATE |  Date and time of starting and ending of crime | Object as datetime | Exclude | Not meaningful for analysis, redundant with REPORT_DAT |

### How the clustering variables relate to the research question

The clustering inputs are 'LATITUDE', 'LONGITUDE', and 'REPORT_DAT'. Latitude and Longitude provides the location of each crime committed and report date implies what time and day of the week the incident occurred. Groups of location can be clustered into regions closer in coordinates like northwest, and southeast regions, and groups of time can be clustered into times closer in hours like early-mornings, afternoons, and late-nights.

### Candidate predictors for classification

To predict each incident's group of cluster, a classifier will be trained. 'OFFENSE', and 'METHOD' will be used to predict crime type and method to present the traits of crimes rather than just the presence of incidents.

### Variables that are likely to be excluded

ID variables such as OBJECTID and OCTO_RECORD_ID provide no meaningful values to analyzing crime patterns, and therefore, will be excluded. 'X', 'Y', 'XBLOCK' and 'YBLOCK' are variables that display map coordinates which are already provided by 'LATITUDE' and 'LONGITUDE'. Due to their redundancy, they are also most likely to be excluded.

### Potential preparation needs

All variables for clustering and classification have no missing values. The variables with missing values are: 
Index(['END_DATE', 'DISTRICT', 'PSA', 'BLOCK_GROUP', 'CENSUS_TRACT', 'BID',
       'OCTO_RECORD_ID'],
      dtype='object'). Since these variables are not going to be used for either clustering or classification, there will be no need to impute for empty the values. 'REPORT_DAT' is stored as text in UTC timezone. It needs adjustment so that UTC datetime is converted to New York timezone before pulling hour and day of week. After deriving hour and day of week, cosine similarity can be helpful to scale and represent time blocks due to 'REPORT_DAT''s cyclical nature. 'LATITUDE' and 'LONGITUDE' need standardization to prevent single variable domination (a huge number of calculated distance overpowering the entire data).









