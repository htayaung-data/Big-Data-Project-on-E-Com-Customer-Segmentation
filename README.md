# Big Data Project - Customer Segmentation for E-commerce Business

## Overview

This project focuses on segmenting customers for an e-commerce business to enhance marketing strategies, improve customer satisfaction, and drive revenue growth. By applying clustering algorithms to comprehensive customer data, we identify distinct segments based on behaviors, preferences, and demographics. The analysis is conducted using PySpark on Amazon EMR, with data stored in AWS S3.


## Project Structure

1. **Data Preprocessing**: Load and clean data from AWS S3, handle missing values, and aggregate features.
2. **Exploratory Data Analysis (EDA)**: Visualize demographics and behaviors to uncover insights.
3. **Feature Engineering**: Encode categorical variables, calculate scores, and create interaction terms.
4. **Feature Scaling and Selection**: Standardize numeric features and select highly correlated attributes.
5. **Clustering Models**: Implement and fine-tune K-Means and GMM to identify customer segments.
6. **Model Validation**: Assess clustering quality using Silhouette, Davies-Bouldin, and Calinski-Harabasz scores.
7. **Cluster Analysis**: Profile each segment to understand characteristics and behaviors.
8. **Business Recommendations**: Provide strategies tailored to each customer segment.
9. **Appendix**: Explore additional segmentation with three clusters to identify high-risk customers.

## Data

The dataset consists of four primary sources, all stored in AWS S3:

- **Behavioral Data**: Customer interactions such as pages viewed and session duration.
- **Transaction Data**: Purchase records including transaction amounts and dates.
- **Engagement Data**: Email opens and clicks indicating customer engagement levels.
- **Demographics Data**: Customer information including age, income, education, and loyalty status.

## Methodology

### 1. Data Preprocessing

- **Loading Data**: Utilized PySpark to read parquet files from AWS S3 into Spark DataFrames.
- **Data Cleaning**: Converted date columns to `DateType`, filled missing values (defaulting "LoyaltyStatus" to "Basic"), and aggregated data by `CustomerID`.
- **Aggregation**: Computed metrics such as total transactions, total spent, average transaction value, engagement scores, and behavioral metrics.

### 2. Exploratory Data Analysis (EDA)

- **Age Distribution**: Analyzed the distribution of customer ages using histograms.
- **Income Distribution**: Visualized income levels to understand economic segments.
- **Loyalty Status Distribution**: Assessed the distribution across different loyalty tiers to gauge engagement levels.

### 3. Feature Engineering

#### 3.1 Categorical Encoding

- **Ordinal Encoding**: Mapped `LoyaltyStatus` and `Education` to numerical values based on their hierarchy.
- **One-Hot Encoding**: Transformed categorical variables like `Gender`, `MarriageStatus`, and `Location` into binary indicators.

#### 3.2 Feature Engineering

- **Recency**: Calculated the number of days since the last purchase.
- **Engagement Score**: Combined metrics from emails opened and clicks to quantify engagement.
- **Behavioral Score**: Integrated page views, session duration, and total sessions to capture behavioral patterns.
- **Spend-Engagement Interaction**: Created interaction terms between total spent and engagement score.
- **Lifetime Value (LTV)**: Estimated customer lifetime value by multiplying average transaction value with total transactions.

#### 3.3 Feature Scaling

- **Standardization**: Applied `StandardScaler` to numeric features to ensure zero mean and unit variance.
- **Vector Assembler**: Combined scaled features into feature vectors for modeling.

### 4. Feature Selection

- **Correlation Analysis**: Selected features with a correlation greater than 0.5 with `total_spent` to reduce multicollinearity. Selected features include:
  - `scaled_total_spent`
  - `scaled_avg_transaction_value`
  - `scaled_total_points_accumulated`
  - `scaled_LoyaltyProgramStatus_encoded`
  - `scaled_engagement_score`
  - `scaled_behavioral_score`
  - `scaled_LTV`

### 5. Clustering Models

#### 5.1 K-Means Clustering

- **Implementation**: Applied K-Means with `k=2` clusters based on initial silhouette analysis.
- **Fine-Tuning**: Adjusted hyperparameters (`maxIter` and `tol`) to optimize clustering performance.

#### 5.2 Gaussian Mixture Model (GMM)

- **Evaluation**: Implemented GMM as an alternative to K-Means but found K-Means to yield better silhouette scores.

#### 5.3 Fine-Tuning and Validation

- **Silhouette Score**: K-Means achieved a Silhouette Score of ~0.727, indicating well-defined clusters.
- **Davies-Bouldin & Calinski-Harabasz Scores**: Further validated clustering quality with favorable metrics.

### 6. Cluster Analysis

- **Numeric Features**: Compared mean values and distributions of key metrics across clusters using box plots.
- **Categorical Features**: Analyzed distributions of `Education` and `LoyaltyStatus` within each cluster.
- **Preferred Categories**: Identified product categories preferred by each cluster to inform targeted marketing.

## Results

### Cluster Profiles

- **Cluster 0 (Upper Tier)**:
  - **Size**: Represents a smaller, highly engaged segment.
  - **Characteristics**: Loyal customers with higher spending, active in loyalty programs, and higher engagement scores.
  
- **Cluster 1 (Lower Tier)**:
  - **Size**: Comprises the majority of the customer base.
  - **Characteristics**: Basic loyalty members with lower spending and engagement levels.

### Model Validation

- **Silhouette Score**: ~0.727, indicating good clustering.
- **Davies-Bouldin Index**: ~0.717, suggesting well-separated clusters.
- **Calinski-Harabasz Index**: ~16,973, reflecting highly distinct clusters.

## Business Recommendations

- **For Upper Tier (Cluster 0)**:
  - **Enhance Loyalty Programs**: Offer exclusive rewards to maintain high engagement.
  - **Personalized Offers**: Tailor promotions to sustain customer satisfaction and loyalty.
  
- **For Lower Tier (Cluster 1)**:
  - **Engagement Campaigns**: Implement targeted marketing to increase interaction.
  - **Loyalty Incentives**: Introduce incentives to encourage upgrades to higher loyalty tiers.
  - **Promotional Strategies**: Design promotions that resonate with this segment's preferences to drive spending.

## Limitations

- **Feature Selection Constraints**: Some valuable features with lower correlations might have been excluded, affecting segmentation granularity.
- **Model Assumptions**: K-Means assumes spherical clusters, which may not capture all customer behavior nuances.
- **Temporal Dynamics**: Analysis relies on historical data and may not account for recent changes in customer behavior.

## Future Work

- **Enhanced Feature Selection**: Incorporate additional features and advanced selection techniques for more refined segments.
- **Alternative Clustering Methods**: Explore algorithms like DBSCAN or hierarchical clustering for potentially more nuanced segments.
- **Temporal Analysis**: Integrate time-series data to capture evolving customer behaviors.
- **Automated Pipelines**: Develop automated data processing pipelines for real-time customer segmentation.

## Tools & Technologies

- **PySpark**: For large-scale data processing and analysis.
- **Amazon EMR**: Managed cluster platform for executing big data frameworks.
- **AWS S3**: Scalable storage service for dataset management.
- **Matplotlib & Seaborn**: Libraries for creating informative visualizations.
- **Scikit-learn**: Utilized for additional evaluation metrics during model validation.
