# Automated Credit Risk Modeling & Scoring System

A comprehensive project demonstrating multi-language (Excel VBA and R) approaches to credit risk, covering:
- **Logistic regression & decision trees for Probability of Default (PD) and Loss Given Default (LGD)**
- **Excel VBA workflows** for capital at risk, historical returns, and prepayment analytics
- **Mortgage analytics & CDS bootstrapping** in R for scenario-based stress tests
- **Regulatory alignment** with SR 11-7 and FRTB guidelines

---

## Table of Contents
1. [Overview](#overview)
2. [R Scripts & tools ](#RScripts&tools)
3. [Methods & Formulas Highlights](#Methods&FormulasHighlights)
4. [Achievements & Observations](#Achievements&Observations)
5. [Repository Structure](#RepositoryStructure)
6. [How to Run](#HowtoRun)
7. [Additional Notes](#AdditionalNotes)

---


--------------------------------------------------------------------------------
## 1. Overview
--------------------------------------------------------------------------------
This project stems from material in CFRM 542: Credit Risk Management. It merges
real-world datasets (consumer loans, mortgage data, and macroeconomic factors)
with advanced modeling techniques across different tools:

- Excel VBA: Ratio analyses, capital at risk (CAR) calculations, and historical
  performance measurement
  
- R: Machine learning for PD/LGD on historical loan data, Mortgage prepayment modeling, 
  BMA calculations, CDS curve bootstrapping, and advanced scenario-based stress testing

The result is a unified framework enabling faster turnaround on risk assessments
and deeper insight into credit exposures under varied conditions.

--------------------------------------------------------------------------------
## 2. R Scripts & tools 
--------------------------------------------------------------------------------
The R scripts (originally in 542_credit_projects_dataandmodel.Rmd) showcase:

• Data Cleaning & Preparation
  - Reads and cleans data from sources such as “fredgraph_data.csv” to remove NA values
  - Filters results to avoid missing values that could skew regression outcomes

• Multivariate Regression (GDP vs. Macro Factors)
  - Regresses GDP growth against factors such as population change, CPI, unemployment, GS10, M2 money supply, and others
  - Checks for outliers (e.g., COVID Q1 2020) and compares model performance with/without these points
  - Uses variance inflation factors (VIF) to guard against multicollinearity
  - Observes changes in R-squared and significance levels upon adding new covariates like M2 or consumer sentiment

    <img width="536" alt="image" src="https://github.com/user-attachments/assets/6a8603af-f9ee-4ea8-8a5a-3baee1739bfe" />


• Logistic Regression for Credit Default
  - Demonstrates an OddsPlot function for visualizing the log-odds of default across binned numeric variables (e.g., revolving utilization)
  - Introduces a K-S statistic function to measure model discrimination (comparing cumulative distributions of predicted probabilities)
  - Builds GLMs (logistic models) using sub-grade, home ownership, loan amount, employment length, and delinquencies to predict default
  - Examines AIC, p-values, and VIF to refine and select the best specification

    <img width="875" alt="image" src="https://github.com/user-attachments/assets/7337528a-7639-4b12-97cc-c6f039653148" />

    <img width="868" alt="image" src="https://github.com/user-attachments/assets/8f27c7c4-c731-4980-87ae-5173a4c3f7e5" />



• Mortgage Cash Flow & BMA Calculations
  - Implements a function (calculate_mortgage_cash_flows) to track monthly balances, defaults, prepayments, and foreclosures
  - Incorporates severity (LGD) and time-to-recovery assumptions, adjusting performing balances and recovered amounts
  - Applies PSA prepayment speed (e.g., 100% PSA, 50% PSA) to vary monthly prepayment rates
  - Calculates net present values based on a spot rate curve (e.g., flat 3% or 4% with semiannual compounding) to measure mortgage valuation
  - Compares results when adjusting interest rates or changing prepayment speeds

• Economic Capital (Vasicek Model)
  - Uses a Vasicek approach for unexpected loss at a given confidence interval (99.9% or 99.97%)
  - Computes a “stressed” probability of default using correlation (rho) and standard normal distributions
  - Subtracts expected losses from the unexpected loss to estimate the required economic capital
  - Illustrates how small changes in PD or correlation can significantly alter capital at high confidence

    <img width="868" alt="image" src="https://github.com/user-attachments/assets/750e8f46-b4d8-40f2-afc5-33cf54009e0a" />


    <img width="868" alt="image" src="https://github.com/user-attachments/assets/7f5a7b23-f17f-47a5-af14-b35425ea1805" />



While the R code focuses on regression, mortgage flows, and advanced modeling, the project also leverages Excel macros (in separate .xlsm files) to:
• Calculate capital at risk for different portfolios
• Automate historical return analysis
• Perform data transformations for prepayment or default logic

<img width="683" alt="image" src="https://github.com/user-attachments/assets/e6ac28f3-2571-4277-9f62-fea064425d10" />

<img width="787" alt="image" src="https://github.com/user-attachments/assets/2bd957d5-b053-4fdf-9afd-8877760f2c58" />

These Excel sheets often contain multiple tabs demonstrating step-by-step calculations, including “BMA examples,” “PSA vs. CDR,” and “VBA macros for scenario pivoting.” Screenshots of these can illustrate how formulas and user-defined macros process large credit datasets, making it more intuitive for finance teams to adapt.

--------------------------------------------------------------------------------
## 3. Methods & Formulas Highlights
--------------------------------------------------------------------------------
### Linear & Logistic Regression
  - Applies standard OLS for GDP growth with macros in R (via lm) and uses glm for binary outcomes like loan default
  - Checks model fit via adjusted R^2 (for OLS) or AIC (for logistic) and inspects residual plots or outliers

### K-S Statistic (Discrimination)
  - Implements a function to sort predicted default probabilities and measure the max gap between cumulative distributions of actual defaulters vs. non-defaulters

### Mortgage Model Formulas
  - Tracks performing balance (PB), new defaults, voluntary prepayments, foreclosure (FCL), and recovers principal over time
  - Uses standard “time to recovery” assumptions for defaulted loans
  - Discounts monthly cash flows at a chosen yield curve or flat spot rate to get present values

### Vasicek Capital at Risk
  - Stressed PD = Φ((Φ⁻¹(PD) + √ρ * Φ⁻¹(confidence)) / √(1-ρ))
  - Unexpected Loss = Stressed PD * LGD
  - Economic Capital = Unexpected Loss – (PD * LGD)

These equations allow the user to test capital requirements at extreme tail risks, consistent with advanced risk management frameworks.

--------------------------------------------------------------------------------
## 4. Achievements & Observations
--------------------------------------------------------------------------------
- Observed strong model performance for default risk with minimal multicollinearity (VIF < 2)
- Improved R-squared (up to ~0.61) when adding M2 money supply changes to GDP regressions
- Automated monthly mortgage valuations, producing net present values near or above the notional principal depending on WAC vs. spot rates
- Showed capital estimates at 99.9% and 99.97% confidence using Vasicek, illustrating how PD and correlation drastically shift required capital

--------------------------------------------------------------------------------
## 5. Repository Structure
--------------------------------------------------------------------------------
Below is a suggested layout. Adjust file and folder names to your actual setup:

• README.txt (this file)  
• excel_vba/
  - Contains .xlsm or .xlsb workbooks with macros for CAR calculations, historical
    returns, or prepayment analytics
• python/
  - Includes scripts such as pd_lgd_model.py for logistic regression and decision
    trees, plus data preprocessing
• r_scripts/
  - Holds R scripts for mortgage cash flow (PSA prepayments, default severity) and
    CDS bootstrapping
• data/
  - Houses CSV or Excel files used across environments (loan data, mortgage data,
    macroeconomic data)
• CODE_OVERVIEW.txt (optional)
  - A text-based summary of the main code logic from each environment

--------------------------------------------------------------------------------
## 6. How to Run
--------------------------------------------------------------------------------
### Excel VBA
- Open the relevant .xlsm file (e.g., CAR_Calculations.xlsm) with macros enabled
- Use the “Developer” tab or press Alt+F8 to run macros (capital at risk, historical
  return modules, etc.)

### R
- Use scripts in the r_scripts/ folder for mortgage or CDS analysis
- Load them into R or RStudio, install any needed libraries (e.g., dplyr, ggplot2)
- Execute line by line or with source("script_name.R"), then inspect printed or
  plotted outputs for monthly cash flows, discount factors, or CDS spread results

--------------------------------------------------------------------------------
## 7. Additional Notes
--------------------------------------------------------------------------------
• Credit Data Source
  - If using public datasets (e.g., Lending Club), ensure data is anonymized or
    partially synthetic

• Mortgage & CDS Assumptions
  - Some parameters (PSA rates, severity, discount rates) follow standard industry
    practice or simplified models for demonstration
  - Real portfolios may require more granular calibrations

• Collaboration
  - Pull requests are welcome for improvements, whether it’s code refactoring,
    new risk metrics, or bug fixes

If you run into issues or have suggestions, feel free to:
• Open an issue in this repository
• Contact me via email or LinkedIn (see profile for details)

--------------------------------------------------------------------------------
