# life_expectancy

This project aims to predict life expectancy using advanced regression models, emphasizing practical deployment strategies and scalability to provide actionable insights into factors influencing life expectancy globally.

## Table of Contents

1. [Data Import](#data-import)
2. [Preprocessing Steps](#preprocessing-steps)
   - [Filling Null Values](#filling-null-values)
   - [Outlier Treatment (Capping method)](#outlier-treatment-capping-method)
   - [Feature Creation](#feature-creation)
   - [Normal Distribution Checking](#normal-distribution-checking)
   - [Correlation Check](#correlation-check)
   - [WOE Transformation](#woe-transformation)
4. [Train Test Splitting](#train-test-splitting)
5. [Model Building](#model-building)
6. [Model Optimization](#model-optimization)
7. [Stacking Classifier](#stacking-classifier)
8. [Univariate Analysis](#univariate-analysis)

## Data Import

The dataset used in this project contains information on whether clients defaulted on their credit card payments. It consists of 30,000 rows and 24 features, along with 1 target column, which is the default column. We have checked the data in terms of statistics using the `describe` command. Finally, the meanings of each columns have been placed in the excel file (second sheet).

## Preprocessing Steps

### Filling Null Values

### Outlier Treatment (Capping method)

Outliers in the dataset were treated using the capping method. The interquartile range (IQR) was calculated for each numeric feature to determine the upper and lower boundaries. For each numeric column, if the values exceeded the upper boundary, they were capped at the upper boundary. Similarly, values below the lower boundary were capped at the lower boundary. This approach ensures that extreme values do not adversely affect the model performance.

### Feature Creation

New features were created to enhance the predictive power of the model by leveraging the relationships between existing features. Specifically, statistical analysis was used to create new columns based on the relationships between `EDUCATION` and `LIMIT_BAL`, as well as `MARRIAGE` and `LIMIT_BAL`.

For each category in `EDUCATION`, new columns were created to capture the mean, sum, minimum, and maximum values of `LIMIT_BAL`. This was done by grouping the data by `EDUCATION` and calculating these statistics, which were then merged back into the main dataset.

Similarly, new columns were created for each category in `MARRIAGE`, capturing the mean, sum, minimum, and maximum values of `LIMIT_BAL` by grouping the data by `MARRIAGE` and performing the same statistical calculations. These new features provide additional insights into the relationships between these variables, which can improve the model's performance.

### Normal Distribution Checking

The normal distribution of numeric columns was assessed using the Kolmogorov-Smirnov test (Smirnov test). This statistical test compares the distribution of each column against a normal distribution hypothesis.

For each numeric column, the Kolmogorov-Smirnov test statistic (`kstest_statistic`) and its associated p-value (`kstest_p_value`) were computed. A p-value greater than 0.05 indicates that the data follows a normal distribution. Columns with p-values less than or equal to 0.05 were identified as not normally distributed.

In cases where the data was not normally distributed, Spearman correlation was chosen instead of Pearson correlation for assessing relationships between variables. Spearman correlation is suitable for ordinal and non-normally distributed data, providing robustness against outliers and non-linear relationships.

This approach ensures that appropriate statistical methods are used depending on the distribution characteristics of the data, thereby enhancing the reliability of the analysis.

### Correlation Check

In this project, two types of correlation analyses were conducted: target correlation and intercorrelation among features.

**Target Correlation:** 
Target correlation assesses the relationship between each feature and the target variable (`default` in this case). Features with an absolute correlation coefficient of 10% or higher with the target are considered to have a significant influence on predicting credit card defaults.

**Intercorrelation Among Features:** 
Intercorrelation examines relationships between pairs of features in the dataset. Features with intercorrelation coefficients of 80% or higher are considered highly correlated. Managing such high intercorrelation helps prevent multicollinearity, ensuring that the model remains robust and interpretable.

**Variance Inflation Factor (VIF):**
Additionally, VIF was used to detect multicollinearity among features. A VIF coefficient below 10 indicates that the variance of a feature is not significantly inflated by its correlations with other features. Features with high VIF scores were carefully reviewed or excluded from the model to enhance its stability and predictive accuracy.

**Selected Columns for Analysis:** 
Based on these analyses, several key features were chosen for further modeling and analysis. These columns strike a balance between their significant correlation with the target variable, moderate intercorrelation with other features, and low VIF scores, ensuring a robust and interpretable predictive model.

This comprehensive approach ensures that the model effectively captures relevant relationships while mitigating issues related to multicollinearity.

### WOE Transformation

WOE transformation plays a crucial role in transforming categorical variables into a form that is more suitable for predictive modeling. Here's how it's implemented:

**Numeric Values:**
- Numerical features are binned based on quartiles (q1, q2, q3).
- Each bin is assigned a category, and WOE values are calculated to capture the relationship between the feature and default status.
- This transformation helps in identifying how each numerical feature contributes to predicting credit card defaults.

**Categorical Values:**
- Categorical features are grouped by their unique values.
- WOE values are computed to quantify the predictive strength of each category in relation to credit card default.
- Missing values and categories with sparse data are handled to ensure robust WOE transformation.

## Train Test Splitting

The dataset is split into training and testing sets to evaluate the performance of the models. Here I have taken 3 inputs depending on the models: Logistic Regression inputs, Catboost custom model inputs declaring categoric features, and inputs for other models.

## Model Building

Several classification models are built to predict the likelihood of credit card defaults, including logistic regression, decision trees, and random forests. Since the dataset is imbalanced, we have to look through gini probabilities, and result is here:

![image](https://github.com/yrovsen/default_credit_card/assets/137065696/405bf9cb-d342-4a16-b92d-4113c253cc04)


## Model Optimization

Hyperparameters of the models are tuned using cross-validation techniques to achieve the best performance. Using optuna library, the models which has performed better has been tuned to get even more better results:

![image](https://github.com/yrovsen/default_credit_card/assets/137065696/8f019e5d-4518-41d0-9383-3ebad2746c41)

 
## Stacking Classifier

A stacking classifier is implemented to combine the predictions of multiple models and improve overall accuracy. As base models, xgboost and lightgbm tuned models, and as a meta model, default tuned catboost model have been used for stacking. However, stacking classifier gini score was below than catboost optuna model which was chosen for univariate analysis as a final model.

## Univariate Analysis

Univariate analysis is performed to understand the distribution and central tendency of individual features. The features which have test gini score more than 20% and are not overfitting chosen for fitting.

![image](https://github.com/yrovsen/default_credit_card/assets/137065696/8762920c-e317-4d5d-89d3-31d722d282e4)

## Results and Conclusion

The models are evaluated based on various metrics, and the best-performing model is identified. The project demonstrates the effectiveness of combining multiple preprocessing and modeling techniques to improve classification accuracy. Regardless of the fact that all models had good results, catboost tuned model has been chosen and using univariate analysis, main features filtered accordingly. Here is ROC&AUC curve and precision-recall trade off curve:

![image](https://github.com/yrovsen/default_credit_card/assets/137065696/66077329-0aab-441f-94f0-a7014d5f5ad6) 
![image](https://github.com/yrovsen/default_credit_card/assets/137065696/2e2e40e2-5939-447d-81e1-71f8defab284)
