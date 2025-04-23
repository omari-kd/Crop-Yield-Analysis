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

</div>
