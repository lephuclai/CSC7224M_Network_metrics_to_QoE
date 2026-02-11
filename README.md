# CSC7224M_Network_metrics_to_QoE

# Video Streaming Startup Delay Analysis

  

Analysis of video streaming startup delays using encrypted network traffic data.

  

## Feature Sets
-  **Outband (F_outband):** Network QoS metrics used for simulation

-  **PCAP (F_inband):** Captured network packet statistics

-  **Chunk (F_chunk):** Video chunk-level data


## Notebooks

  

### 1. `StartupDelayPrediction.ipynb` (Regression)

Predicts continuous startup delay values.

- Data preprocessing and feature engineering

- Feature visibility scenarios (Meta-only, PCAP, Chunk-based)

- Linear Regression baseline

- Random Forest with hyperparameter tuning

- Performance comparison across feature sets

- SHAP interpretability analysis

  

**Models:** Linear Regression, Random Forest

**Output:** Predicted delay in seconds

  

### 2. `StartupDelayClassification.ipynb` (Classification)

Detects timeout events and analyzes startup failure risk.

- Data preprocessing (same pipeline as above)

- Binary timeout label definition

- Baseline classification model

- Feature visibility impact on timeout detection

- Survival analysis with Cox Proportional Hazards Model

- Risk stratification by network conditions

  

**Models:** Random Forest Classifier, Cox PH Model

**Output:** Timeout/success classification + survival analysis

### 3. `Phuc_Lai_Network_to_QoE.ipynb`
This note book includes some of the presented analysis and results. The section names match those used in the notebook.
#### 3.1 Data Exploration
- Investigated each feature set using box plots, histograms, and feature correlations, etc.
- Plotted the distribution of selected features against the target variables (stall, QoE score, etc.).

#### 3.2 Dimensionality Reduction 
- Investigated whether reducing the number of features in the `F_inband` feature set is effective.
- Performed Dimensionality Reduction using SVD.
- Built a linear regression model and calculated the MSE for each number of components and compared their MSE to investigate the effectiveness of Dimensionality Reduction.

#### 3.3 QoE Score Prediction
- Built models to predict the user-perceived MOS score.
- **Models trained**:
  - Regression with Neural Network
  - Classification with Neural Network
  - Classification with XGBoost  
- Feature sets used: F_outband and F_inband
- **Observations:** The regression neural network suffered from under-predicting low MOS values and over-predicting high MOS values. It also tended to predict the average MOS.
- **Conclusion:** Therefore, it is more suitable to treat this as a classification problem, aiming for high recall on sessions with bad QoE (1 to 2.9).

#### 3.4 Other results:
- Section **Scale the data**: Performed linear regression with scaled dataset to check the importance of features
- Section **Linear regression**: Some of the results from this section are used to compare the performance of Neural Network, XGBoost model


  
### 4. `Martim_NetworkTrafficToQoE.ipynb`
#### 4.1 Data Exploration
- This notebook includes some of our data exploration mentioned in the final report: Number of Switches vs RTT bins, Average Resolution Quality per Category

#### 4.2 Stall Classification
- Trained a Random Forest classifier, a Neural Network, and an XGBoost classifier for binary classification (0 = No Stall and 1 = Has stall)
- Investigated which features are mainly responsible for stalls
- Trained the models using 2 different feature sets:
  - ISP view: Hand-selected some of the most important feature in F_outband and F_inband
  - Full view: ISP view + chunks data extracted from browser
  - Insight: ISP view ⋍ Full view, but F1 precision and F1 score for full view were better
- Analyzed feature importance, survival analysis of Rate vs Download Bandwidth
- Performed Principal Component Analysis (PCA) to see if the model can be improve.
  - Insights: Model could not be improved more than the achieved results because it is hard to distinguish between stall and non-stall session in the dataset, and other factor, such as buffer health of client, affect stall