
## README

### Project Overview

This project involves analyzing the relationship between various socio-economic indicators and the Human Development Index (HDI) using a dataset containing information about different countries. The primary objective is to explore and visualize how different variables influence the HDI and perform statistical tests to validate the findings.

### Dataset

The dataset used in this project includes the following columns:
- `country`: Name of the country
- `region`: Region of the country
- `hdicode`: HDI category of the country
- `hdi_2000`: HDI of the country in 2000
- `hdi_2021`: HDI of the country in 2021
- `le_1990`: Life expectancy in 1990
- `gnipc_1990`: Gross National Income per capita in 1990
- `gnipc_2021`: Gross National Income per capita in 2021

### Project Structure

The project is structured into various sections to address different analysis objectives:

1. **Data Cleaning and Preparation**:
   - Load and clean the dataset.
   - Replace missing values and standardize column names.
   - Ensure qualitative columns are correctly labeled.

2. **Exploratory Data Analysis (EDA)**:
   - Univariate analysis: Summary statistics, histograms, and boxplots.
   - Bivariate analysis: Scatter plots, correlation coefficients, and regression models.

3. **Statistical Analysis**:
   - Correlation and regression analysis between different pairs of quantitative variables.
   - Analysis of variance (ANOVA) and eta-squared for quantitative vs. qualitative variables.
   - Chi-squared tests for independence between qualitative variables.

### Instructions

#### Data Cleaning and Preparation

1. **Load the Data**:
    ```r
    hdi_data <- read.csv('HDI.csv')
    ```

2. **Clean and Prepare Data**:
    ```r
    hdi_data$region <- gsub("EAP", "East Asia and Pacific", hdi_data$region)
    hdi_data$region <- gsub("ECA", "Europe and Central Asia", hdi_data$region)
    hdi_data$region <- gsub("LAC", "Latin America & the Caribbean", hdi_data$region)
    hdi_data$region <- gsub("MNA.", "Middle East and North Africa", hdi_data$region)
    hdi_data$region <- gsub("SAR", "South Asia", hdi_data$region)
    hdi_data$region <- gsub("SSA", "Sub-Saharan Africa", hdi_data$region)
    hdi_data$region <- gsub("SA", "South Asia", hdi_data$region)
    hdi_data$region <- gsub("AS", "Asia", hdi_data$region)
    hdi_data <- hdi_data %>%
      filter(!is.na(region) & region != "" & !is.na(hdi_2021) & hdi_2021 != "")
    ```

#### Exploratory Data Analysis (EDA)

1. **Univariate Analysis**:
    - Boxplot for `gnipc_1990`:
        ```r
        ggplot(hdi_data, aes(y = gnipc_1990)) +
          geom_boxplot(fill = "lightgreen") +
          labs(title = "Boxplot du RNB par habitant en 1990", y = "RNB par habitant en 1990 ($)") +
          theme_minimal()
        ```
    - Histogram for `hdi_2021`:
        ```r
        ggplot(hdi_data, aes(x = hdi_2021)) +
          geom_histogram(binwidth = 0.05, fill = "steelblue", color = "black") +
          labs(title = "Distribution de l'IDH en 2021", x = "IDH en 2021", y = "Nombre de Pays") +
          theme_minimal()
        ```

2. **Bivariate Analysis**:
    - Scatter plot and regression line for `le_1990` vs `hdi_2021`:
        ```r
        ggplot(hdi_data, aes(x = le_1990, y = hdi_2021)) +
          geom_point(color = "#69b3a2", alpha = 0.7) +
          geom_smooth(method = "lm", se = FALSE, color = "blue", linetype = "dashed") +
          labs(title = "Nuage de points et droite des moindres carrés entre l'espérance de vie en 1990 et l'IDH en 2021", 
               x = "Espérance de vie en 1990 (années)", 
               y = "IDH en 2021") +
          theme_minimal()
        ```

#### Statistical Analysis

1. **Correlation and Regression Analysis**:
    - Calculate Pearson correlation coefficient:
        ```r
        correlation <- cor(hdi_data$le_1990, hdi_data$hdi_2021, use = "complete.obs")
        cat("Le coefficient de corrélation linéaire entre le_1990 et hdi_2021 est :", correlation, "\n")
        ```

    - Linear regression model and diagnostic plots:
        ```r
        model <- lm(hdi_2021 ~ le_1990, data = hdi_data)
        summary_model <- summary(model)
        print(summary_model)
        par(mfrow = c(2, 2))
        plot(model)
        ```

2. **ANOVA and Eta-Squared**:
    - ANOVA and Eta-Squared for `region` and `hdi_2021`:
        ```r
        model_aov <- aov(hdi_2021 ~ region, data = hdi_data)
        anova_result <- anova(model_aov)
        ss_total <- sum(anova(model_aov)$"Sum Sq")
        ss_explained <- anova(model_aov)$"Sum Sq"[1]
        eta_squared <- ss_explained / ss_total
        cat("Eta squared pour l'IDH 2021 par région est :", eta_squared, "\n")
        ```

3. **Chi-Squared Test**:
    - Chi-squared test for independence between `region` and `hdicode`:
        ```r
        contingency_table <- table(hdi_data$region, hdi_data$hdicode)
        chi2_test <- chisq.test(contingency_table)
        print(chi2_test)
        ```

### Conclusion

This project provides a comprehensive analysis of how various socio-economic indicators relate to the Human Development Index. By following the steps outlined, you can replicate the analysis and gain insights into the factors influencing HDI across different countries and regions.

### Requirements

- R
- R packages: `ggplot2`, `dplyr`, `car`

### How to Run

1. Install the required R packages:
    ```r
    install.packages(c("ggplot2", "dplyr", "car"))
    ```

2. Load and run the R script containing the code provided in this README.

### Contact

For any questions or further information, please contact mouadelkbabty@gmail.com .

