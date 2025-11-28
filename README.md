# Forest Fire Impact Prediction: GLM & GAM Analysis

![R](https://img.shields.io/badge/Language-R-blue)
![Models](https://img.shields.io/badge/Models-GLM%20%7C%20GAM-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

> **Modeling the burned area of forest fires in Montesinho Natural Park using Generalized Additive Models to capture non-linear climatic dependencies.**

## Project Overview
Predicting the severity of forest fires is a complex regression problem due to the stochastic nature of fire spread and the extreme skewness of the target variable (burned area). This project analyzes meteorological data (FWI indices, temperature, wind) to model fire impact.

The study moves beyond simple Linear Regression, addressing **heteroscedasticity** and **non-normality** by implementing **Generalized Linear Models (GLMs)** and **Generalized Additive Models (GAMs)** with spline functions.

[**📄 View Full HTML Report**](docs/full_report.html)

## Methodology & Key Challenges

### 1. Data Preprocessing & EDA
* **Skewness Handling:** The target variable `area` was extremely right-skewed with a heavy zero-inflation (many small fires). Applied a $log(x+1)$ transformation to normalize the distribution.
* **Correlation Analysis:** Analyzed the Fire Weather Index (FWI) components (FFMC, DMC, DC, ISI) to identify multicollinearity.

### 2. Modeling Strategy
We compared three approaches to capture the relationship between weather and fire size:
* **Linear Regression:** Served as a baseline, though limited by rigid linearity assumptions.
* **Generalized Linear Models (GLM):** Experimented with **Gamma** and **Inverse Gaussian** families to better model the positive, continuous nature of the data without forced log-normalization.
* **Generalized Additive Models (GAM):** Implemented using the `mgcv` library. Used **smoothing splines** to capture non-linear threshold effects (e.g., wind speed only impacting fire size after a certain velocity).

## Tech Stack
* **Language:** R
* **Modeling Libraries:** `mgcv` (for GAMs), `caret`
* **Data Manipulation:** `tidyverse` (dplyr, tidyr)
* **Visualization:** `ggplot2`, `corrplot`

**Key Findings:**
* **The "Outlier" Problem:** Meteorological data alone has limited predictive power ($R^2$ remains low) because massive fires are often driven by specific local conditions (fuel layout, slope) not present in the dataset.
* **Non-Linearity:** The GAM analysis revealed that `temp` and `DC` (Drought Code) have non-linear relationships with fire size, which linear models failed to capture.

## Installation & Usage

1.  **Clone the repo:**
    ```bash
    git clone [https://github.com/gonzalo-cruz/forest-fire-regression.git](https://github.com/gonzalo-cruz/forest-fire-regression.git)
    ```
2.  **Open the Analysis:**
    Open `analysis/regression_modeling.Rmd` in RStudio.
3.  **Run the Models:**
    Ensure you have the required libraries installed:
    ```r
    install.packages(c("tidyverse", "mgcv", "corrplot", "ggthemes"))
    ```

## Engineering Decisions
* **Why GAMs?** We chose **GAMs** for *interpretability*. In a scientific/forestry context, it is crucial to understand *how* specific variables (like RH or Wind) affect the output curve, rather than just getting a black-box prediction.
* **Handling Zero-Values:** The dataset contained many small fires ($area \approx 0$). We addressed this by shifting the distribution ($log(x+1)$) to allow for valid mathematical operations in models that require positive inputs.

## Authors
* **Gonzalo Cruz Gómez**
* **Marc Gil Arnau**
* **Yasmin Ezzarqtouni Marzoug**

---
*Data Source: Cortez, P., & Morais, A. (2007). A Data Mining Approach to Predict Forest Fires using Meteorological Data.*
