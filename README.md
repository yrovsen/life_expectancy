# Life Expectancy Regression Models

This project aims to predict life expectancy using advanced regression models, emphasizing practical deployment strategies and scalability to provide actionable insights into factors influencing life expectancy globally.

## Table of Contents

1. [Data Import](#data-import)
2. [Preprocessing Steps](#preprocessing-steps)
   - [Filling Null Values](#filling-null-values)
   - [Outlier Treatment (Capping method)](#outlier-treatment-capping-method)
   - [Feature Creation](#feature-creation)
   - [Normal Distribution Checking](#normal-distribution-checking)
   - [Correlation Check](#correlation-check)
   - [Scaling](#scaling)
4. [Train Test Splitting](#train-test-splitting)
5. [Model Building](#model-building)
6. [Model Optimization](#model-optimization)
7. [Stacking Regressor](#stacking-regressor)
8. [Univariate Analysis](#univariate-analysis)

## Data Import

The dataset used contains demographic, socioeconomic, and health-related indicators from various countries, with life expectancy as the target variable. It consists of 2938 rows and 21 features, along with 1 target column, which is the Life expectancy column. We have checked the data in terms of statistics using the `describe` command. Finally, the meanings of each columns have been placed in the excel file (second sheet).

## Preprocessing Steps

### Filling Null Values

Missing values in the dataset were filled using appropriate methods such as mean for numeric columns and mode for categoric columns based on feature distributions.

### Outlier Treatment (Capping method)

Outliers in the dataset were treated using the capping method. The interquartile range (IQR) was calculated for each numeric feature to determine the upper and lower boundaries. For each numeric column, if the values exceeded the upper boundary, they were capped at the upper boundary. Similarly, values below the lower boundary were capped at the lower boundary. This approach ensures that extreme values do not adversely affect the model performance.

### Feature Creation

New columns are created to explore the relationship between 'Status' and 'infant deaths' using various statistical measures. The code iterates through predefined statistical methods (`statistics`) to compute aggregates of 'infant deaths' grouped by 'Status'. Resulting columns are appended to the dataset to capture these relationships.

Similarly, new columns are generated to analyze the association between 'Status' and 'Total expenditure' using statistical measures defined in `statistics`. This process enhances the dataset by incorporating insights into how 'Total expenditure' varies across different 'Status' categories.

These engineered features aim to enrich predictive models by encapsulating significant relationships between predictors and 'Status', contributing to a more nuanced understanding of factors influencing life expectancy.

### Normal Distribution Checking

The normal distribution of numeric columns was assessed using the Kolmogorov-Smirnov test (Smirnov test). This statistical test compares the distribution of each column against a normal distribution hypothesis.

For each numeric column, the Kolmogorov-Smirnov test statistic (`kstest_statistic`) and its associated p-value (`kstest_p_value`) were computed. A p-value greater than 0.05 indicates that the data follows a normal distribution. Columns with p-values less than or equal to 0.05 were identified as not normally distributed.

In cases where the data was not normally distributed, Spearman correlation was chosen instead of Pearson correlation for assessing relationships between variables. Spearman correlation is suitable for ordinal and non-normally distributed data, providing robustness against outliers and non-linear relationships.

This approach ensures that appropriate statistical methods are used depending on the distribution characteristics of the data, thereby enhancing the reliability of the analysis.

### Correlation Check

In this project, two types of correlation analyses were conducted: target correlation and intercorrelation among features.

**Target Correlation:** 
Target correlation assesses the relationship between each feature and the target variable (`Life expectancy` in this case). Features with an absolute correlation coefficient of 40% or higher with the target are considered to have a significant influence on predicting life expectancy.

**Intercorrelation Among Features:** 
Intercorrelation examines relationships between pairs of features in the dataset. Features with intercorrelation coefficients of 80% or higher are considered highly correlated. Managing such high intercorrelation helps prevent multicollinearity, ensuring that the model remains robust and interpretable.

**Variance Inflation Factor (VIF):**
Additionally, VIF was used to detect multicollinearity among features. A VIF coefficient below 10 indicates that the variance of a feature is not significantly inflated by its correlations with other features. Features with high VIF scores were carefully reviewed or excluded from the model to enhance its stability and predictive accuracy.

**Selected Columns for Analysis:** 
Based on these analyses, several key features were chosen for further modeling and analysis. These columns strike a balance between their significant correlation with the target variable, moderate intercorrelation with other features, and low VIF scores, ensuring a robust and interpretable predictive model.

This comprehensive approach ensures that the model effectively captures relevant relationships while mitigating issues related to multicollinearity.

### Scaling

Standard scaler used for bringing features to the same range for better performance.

## Train Test Splitting

The dataset is split into training and testing sets to evaluate the performance of the models. Here I have taken 3 inputs depending on the models: Linear Regression inputs, Catboost custom model inputs declaring categoric features, and inputs for other models.

## Model Building

Several Regression models are built to predict the life expectancy, including linear regression, random forest regressor, SVR and so on. In order to observe overfitting in models, both train and test R2 scores have been shown for all models in one dataframe.


## Model Optimization

Hyperparameters of the models are tuned using cross-validation techniques to achieve the best performance. Using optuna library, the models which has performed better has been tuned to get even more better results:

![image](https://github.com/yrovsen/life_expectancy/assets/137065696/8b9ea9a0-98e5-42d0-8d7a-1cb4ef67e5bc)

## Stacking Regressor

A stacking classifier is implemented to combine the predictions of multiple models and improve overall accuracy. As base models, catboost default and lightgbm tuned models, and as a meta model, xgboost tuned model have been used for stacking. However, stacking regressor R2 score was below than xgboost optuna model which was chosen for univariate analysis as a final model.

## Univariate Analysis

Univariate analysis is performed to understand the distribution and central tendency of individual features. The features which have test r2_score more than 35% and are not overfitting chosen for fitting.

## Results and Conclusion

The models are evaluated based on various metrics, and the best-performing model is identified. The project demonstrates the effectiveness of combining multiple preprocessing and modeling techniques to improve accuracy. Regardless of the fact that all models had good results, catboost tuned model has been chosen and using univariate analysis, main features filtered accordingly.


![image](https://github.com/yrovsen/life_expectancy/assets/137065696/1af34e5b-07a6-46dc-8699-671aa27b31cc)

