# HR Analytics: Workforce Retention and Employee Turnover Analysis

**Author:** Hoang Do Phan  
**Date:** May 2026  
**Project Type:** Portfolio Data Analytics Project  
**Tools:** R + Tableau  

---

# 1. Business Understanding

## Background

Employee turnover is an important workforce challenge that can affect productivity, team stability, and long-term organizational performance. When employees leave, organizations may face additional costs related to hiring, training, and knowledge replacement.

This project uses HR analytics to explore workforce retention patterns and identify factors that may be associated with employee turnover. The analysis focuses on employee satisfaction, salary level, workload, promotion history, department, and attrition status.

Unlike the Python and Power BI version, this version uses R for data preparation and analysis, and Tableau for dashboard visualization and storytelling.

---

## Business Problem

High employee turnover can create operational challenges and increase workforce management costs.

The goal of this analysis is to better understand which employee groups are more likely to leave and what workplace factors may be connected to attrition.

---

## Project Goal

The goal of this project is to analyze employee attrition patterns and develop insights that can support employee retention strategies.

The final output will include cleaned data, exploratory analysis, Tableau visualizations, and business recommendations.

---

## Key Questions

1. Which employee groups have higher attrition rates?
2. How does employee satisfaction differ between employees who stayed and employees who left?
3. Does salary level influence employee turnover?
4. How does promotion history relate to retention?
5. Which departments may require stronger retention efforts?

---

## Stakeholders

- Human Resources Team
- Department Managers
- Workforce Planning Team
- Business Leadership Team
- Talent Management Team

---

# 2. Data Understanding

## Data Source

This project uses a publicly available HR Employee Attrition dataset. The dataset is anonymized and does not identify a specific company.

It includes employee-related variables that can be used to explore workforce trends, employee satisfaction, workload, compensation, promotion history, and attrition status.

---

## Dataset Features

| Variable | Description |
|---|---|
| satisfaction_level | Employee satisfaction score |
| last_evaluation | Most recent performance evaluation score |
| number_project | Number of projects assigned to the employee |
| average_montly_hours | Average monthly working hours |
| time_spend_company | Number of years the employee has spent at the company |
| Work_accident | Whether the employee had a work accident |
| left | Employee attrition indicator |
| promotion_last_5years | Whether the employee was promoted in the last five years |
| Department | Employee department |
| salary | Salary category |

---

## Dataset Scope

The dataset supports analysis of employee turnover patterns by combining satisfaction, workload, compensation, promotion, and department-level information.

The `left` variable is used as the main outcome variable:

- `0` = Employee stayed
- `1` = Employee left

---

## Dataset Limitations

- The dataset does not include company name or industry information.
- Employee demographic variables such as age, gender, and education level are not included.
- The dataset does not contain qualitative reasons for resignation.
- External labor market factors are not included.
- Results should be interpreted as exploratory rather than fully causal.

---

## Data Quality Considerations

The dataset is useful for educational and portfolio-based HR analytics work because it includes key variables related to employee retention.

However, because the dataset is anonymized and does not provide full organizational context, the analysis focuses on identifying patterns and relationships rather than making final causal claims.
# 3. Data Preparation

## Overview

In this step, the HR dataset was imported into R and prepared for analysis. The preparation process included loading packages, uploading the dataset, checking data structure, identifying missing values, removing duplicate records, and creating new variables for analysis and Tableau visualization.
## 3.1 Load Required Packages
```r
# Install packages if needed
install.packages("tidyverse")
install.packages("janitor")
install.packages("readr")

# Load tidyverse for data cleaning, transformation, and analysis
library(tidyverse)

# Load janitor for checking and cleaning column names
library(janitor)

# Load readr for importing and exporting CSV files
library(readr)
```
The following R packages were used for data import, cleaning, transformation, and summary analysis.
## 3.2 Import Dataset

The HR Employee Attrition dataset was uploaded and imported into R for preparation and analysis.
```r
# Upload CSV file in Google Colab or R environment
uploaded_file <- file.choose()

# Read the HR dataset
hr_data <- read_csv(uploaded_file)

# Preview the first few rows
head(hr_data)
```
