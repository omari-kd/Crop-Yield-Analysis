# Crop-Yield-Analysis

<div>

Crop yield is influenced by several factors, including temperature, NPK (Nitrogen, Phosphorus, Potassium) levels in the soil and fertilizer application. Optimal Temperature ranges vary depending on the crop, but generally, excessive heat or cold can negatively impact growth and yield.

NPK levels play a crucial role in plant nutrition, with nitrogen promoting vegetative growth, Phosphorus supporting root development and flowering and Potassium enhancing overall plant health and disease resistance.

Fertilizer application can improve crop yield by replenishing essential nutrients in the soil, but its important to use the right type and amount of fertilizer for the specific crop and soil conditions.

### **Dataset Overview**

The dataset contains 109 observations of 7 variables. These variables include:

1.  Rainfall in mm
2.  Temperature, possibly in degree celsius
3.  Fertilizer, presumably the amount of NPK fertilizer applied
4.  Nitrogen (N), soil nitrogen level before fertilization
5.  Phosphorus (P), soil phosphorus level
6.  Potassium (K), soil potassium level
7.  Yield in quantity per acre (q/acre), that is crop output

Since no additional document is provided, assuming:

-   Fertilizer is the total NPK fertilizer applied, rather than broken down into individual N, P, K values

-   N, P, K are baseline soil nutrient levels before any fertilizer is added

### **Data Preparation**

Handling Missing Data

A threshold was defined to know the percentage of missing data. All columns had about 9.17% or less NA values.

The missing data was visualised using the naniar package and ggplot

![Image](https://github.com/user-attachments/assets/53d40d17-65b4-41e0-8411-90446b7445c2)

![Image](https://github.com/user-attachments/assets/ee9d92e9-1a3e-4eb8-b6cd-4e781c04ecfd)

The visualisations shows the percentage of missing values for each variable in the dataset. However, the visualisations alone cannot directly determine whether the data is Missing Completely at Random (MCAR), Missing at Random (MAR) or Missing Not at Random (MAR). To determine the missing data mechanism, statistical tests and analyses were performed.

Results from TestMCARNormality indicates more than one missing data pattern should be present, meaning that only one missing data pattern exists in the dataset, which confirms the data visualisations of missing data. Using the "which" function, exactly 10 row numbers are seen, confirming same rows, same variables = 1 pattern. It can reasonably be assume MCAR. The practical choice is to use listwise deletion (the missing data is a small amount) or multiple imputation using missForest (Random Forest Imputation). Listwise deletion was chosen as the method for handling missing data.

### **Exploratory Data Analysis**

**Summary Statistics of Yield**

The minimum and maximum yield are 5.5 and 12 q/acre respectively. 25% of the yield (1st quartile) is below 7 q/acre and 75% of the yield (3rd quartile) is below 11 q/acre or 25% of the yield is above 11 q/acre, whilst the average yield is 9 q/acre.

The data distribution is slightly positively skewed from the comparison values of the median and mean.

**Summary Statistics of Rainfall**

The minimum and maximum rainfall are 400 and 1300 mm respectively. 25% of the rainfall (1st quartile) is below 450 mm and 75% of the rainfall (3rd quartile) is below 1237 mm, whilst the average rainfall is 849 mm.

The rainfall distribution is negatively skewed from the comparison values of the median and mean.

**Data Visualisation of Yield vs Rainfall Using Scatter Plot**

![Image](https://github.com/user-attachments/assets/f9ac09e3-d5ee-471f-a6a4-ea37ffa855ef)

From the plot, there is a positive correlation, indicated by the upward red regression line. As rainfall increases, yield tends to increase. The rainfall values seem to be clustered into two distinct groups, one around 400mm to 500mm and the other around 1200mm. Despite this, the trend suggests more rainfall generally supports higher yields.

**Data Visualisation of Yield vs Temperature Using Scatter Plot**

![Image](https://github.com/user-attachments/assets/e5a378f4-03be-4c0e-91cb-a6562582ae5b)

The plot indicates a negative correlation, indicated by the downward red regression line. As temperature increases, yield tends to decrease. The yield is highest when temperatures are lower (below -30 ) suggesting crops might be sensitive to heat.

**Data Visualisation of Yield by Rainfall and Temperature**

![Image](https://github.com/user-attachments/assets/459396a1-d20e-47b4-8bd3-e858484ece39)

The plot reinforces the first plot's trend (more rainfall = higher yield). The red dots (higher temperature) are mostly on the left side (low rainfall) and these are associated with lower yields. Blue/purple dots (cooler temperature) are on the right side (high rainfall) and these are associated with higher yields.

This combined plot indicates that both high rainfall and lower temperature are associated with better yields.

High temperature and low rainfall significantly reduce yield.

**Data Visualisation of Mean Yield vs Fertilizer**

![Image](https://github.com/user-attachments/assets/118029cb-33bd-4b48-ab84-3f100edc0711)

The plot clearly shows an upward trend, average yield increases as fertilizer use increases. From 50 to 70 units of fertilizer, yield gradually improves. Beyond 70, there is a more noticeable jump and stabilization at higher yields(10 - 11 Q/acre).

Fertilizer positively influences yield, especially when moving from moderate (\~60) to higher levels. However, there's some fluctuation, suggesting other factors (like rainfall or temperature) may also be playing a role.

**Data Visualisation of Yield by Fertilizer and Rainfall**

![Image](https://github.com/user-attachments/assets/96ca4b18-4b40-42b5-84be-6e9570ba6662)

The plot shows the visualisation of yield by fertilizer and rainfall. The light blue indicates low rainfall (400 mm) and the dark blue indicates high rainfall (\~1200 mm).

At all fertilizer levels, higher yields occurs when rainfall is high (dark blue). Even at higher fertilizer levels, yield is low if rainfall is low (light blue). High Fertilizer and high rainfall give the best yields.

Rainfall is a critical factor. Without sufficient rainfall, even high fertilizer does not significantly increase yield.

**Diminishing Returns of the fertilizer**

The plot below gives an important insights into the nature of the fertilizer effect on yield and it sets the stage nicely for a more sophisticated model.

![Image](https://github.com/user-attachments/assets/10c86a71-b7af-46e5-80e5-8245622ba3b9)

The scatter plot with a polynomial regression line (blue), shows a non-linear relationship between fertilizer and yield.

The relationship is increasing but non-linear, looks like a quadratic or near-logistic shape. Initially as fertilizer increases, yield increases slowly. After a threshold (\~60–65), the rate of increase in yield accelerates. There may be signs of diminishing returns beyond \~75–80, where the yield gain per unit fertilizer starts to level off.

Yield responds positively to fertilizer, but the efficiency of additional fertilizer decreases at higher levels; classic diminishing returns.

**Modelling**\
**Linear Regression Model**

![Image](https://github.com/user-attachments/assets/19a5b7eb-20d4-4938-9a99-58e0287cbcdb)

-   R-Squared =0.8856 means that about 88% of the variability in the target variable (crop yield) is explained by the features in the regression model and is typically considered a good fit.

-   Temperature has a strong negative effect (p \< 0.05).

-   Nitrogen and Potassium are significant positive contributors (p \< 0.05)

-   The effect of Phosphorus is non statistically significant (p = 0.088 \> 0.05), the result was close to significance.

-   Fertilizer and Rainfall are not significant individually.

**Decision Tree Model**

![Image](https://github.com/user-attachments/assets/0b519a03-e05c-41e1-896e-82e38805483c)

The root node of the decision is the first split and shows the most influential feature in predicting crop yield. In this model, rainfall is at the root, meaning it has the highest information gain and is the most important factor in determining crop yield. The subsequent splits provide additional insights into how different combination of features influence the prediction of crop yield.

**RandomForest Model**

![Image](https://github.com/user-attachments/assets/0123f534-c1bf-4407-8568-b42eebf6aebf)

**Variable Important Metrics**

**%IncMSE:** This tells how much the mean squared error increases when that variable is permuted; higher means more important.

**IncNodePurity:** Measures how much a variable improves node purity (i.e., splits that reduce variance); also higher means more important.

Based on the output, temperature is the most impactful predictor of crop yield. Rainfall and Potassium are also strong predictors. Fertilizer has the least influence in the randomForest model.\

**For better visualisation than the VarImpPlot()**

![Image](https://github.com/user-attachments/assets/c73c9de5-13b2-4735-beb3-a83ac5a87c51)

\
\
**Model Performance**

**Conclusion**

1.  Rainfall has a strong positive linear effect on yield.

2.  Temperature has a negative linear effect on yield.

3.  Fertilizer has a positive non-linear effect (possibly quadratic or logistic).

4.  Yield is maximized under high rainfall, moderate temperature and high but not extreme fertilizer input.

</div>
