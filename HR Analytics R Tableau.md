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
<img width="831" height="470" alt="Screenshot 2026-05-25 133809" src="https://github.com/user-attachments/assets/c450bc14-1a80-48dc-8d8c-5025fa85111a" />
## 3.3 Inspect Dataset Structure

The dataset structure was reviewed to understand the number of rows, columns, variable names, and data types.
```r
# Check dataset dimensions
dim(hr_data)

# View column names
colnames(hr_data)

# Check data types and structure
str(hr_data)

# Summary statistics
summary(hr_data)
```
<img width="729" height="148" alt="Screenshot 2026-05-25 134035" src="https://github.com/user-attachments/assets/dbc763fa-deab-44fc-97ab-74507e4b110a" />
<img width="820" height="510" alt="Screenshot 2026-05-25 134026" src="https://github.com/user-attachments/assets/dacebc35-1ff8-42b1-a913-f8999aead247" />
<img width="811" height="403" alt="Screenshot 2026-05-25 134101" src="https://github.com/user-attachments/assets/3271d1ec-a7ba-4424-a9ac-d1c7cbe26d42" />
## 3.4 Check Missing Values

Missing values were checked across all columns to determine whether additional cleaning was required.
```r
# Count missing values by column
colSums(is.na(hr_data))
```
<img width="757" height="193" alt="Screenshot 2026-05-25 134344" src="https://github.com/user-attachments/assets/46c05e9f-3e55-4c53-bee3-50424d716236" />
## 3.5 Check Duplicate Records

Duplicate records were checked because repeated rows can affect the accuracy of analysis and dashboard metrics.
```r
# Count duplicate rows
sum(duplicated(hr_data))
# Remove duplicate rows
hr_cleaned <- hr_data %>%
  distinct()

# Check updated dimensions after removing duplicates
dim(hr_cleaned)
```
<img width="376" height="176" alt="Screenshot 2026-05-25 134545" src="https://github.com/user-attachments/assets/07b2e1ec-9c5e-41c2-b50d-21f3e1009b0f" />
## 3.6 Clean and Standardize Column Names

Column names were reviewed and standardized to improve readability and make the dataset easier to work with in R and Tableau.
```r
# Clean column names
hr_cleaned <- hr_cleaned %>%
  clean_names()

# View updated column names
colnames(hr_cleaned)
```
<img width="765" height="205" alt="Screenshot 2026-05-25 134817" src="https://github.com/user-attachments/assets/2875f0a7-fa34-4032-a22a-b6e9deb6e4dd" />
## 3.7 Create Analytical Variables

New descriptive variables were created to make the analysis and Tableau dashboard easier to understand.
```r
# Create descriptive variables for analysis and dashboarding
hr_cleaned <- hr_cleaned %>%
  mutate(
    attrition_status = if_else(left == 1, "Left", "Stayed"),
    promotion_status = if_else(promotion_last_5years == 1, "Promoted", "No Promotion"),
    work_accident_status = if_else(work_accident == 1, "Accident", "No Accident")
  )

# Preview updated dataset
head(hr_cleaned)
```
<img width="819" height="429" alt="Screenshot 2026-05-25 134924" src="https://github.com/user-attachments/assets/b9a12fd8-3e6d-4c4f-9399-b94c94b1e52d" />

### Results

New categorical variables were created to improve data interpretation and visualization. These variables provide clear labels for employee attrition, promotion history, and work accident status, making the dataset more suitable for analysis and Tableau dashboard development.
## 3.8 Verify Category Values

Categorical variables were checked to confirm available groups and ensure data consistency.
```r
# Check department categories
unique(hr_cleaned$department)

# Check salary categories
unique(hr_cleaned$salary)

# Check attrition status categories
unique(hr_cleaned$attrition_status)
```
<img width="755" height="168" alt="Screenshot 2026-05-25 135141" src="https://github.com/user-attachments/assets/c1a230f1-e0f2-4296-ae3c-494ca0e9bdb2" />
# 4. Exploratory Data Analysis

## Overview

The purpose of exploratory data analysis (EDA) is to identify patterns, trends, and relationships within the HR dataset.

This section focuses on understanding employee attrition and examining how factors such as department, salary level, employee satisfaction, workload, and promotion history may influence workforce retention.

The findings from this analysis will be used to support the Tableau dashboard and develop business recommendations.
## 4.1 Attrition Overview

This section examines the overall employee attrition rate.
```r
# Calculate total employees, employees left, and attrition rate
total_employees <- nrow(hr_cleaned)

employees_left <- sum(hr_cleaned$left)

attrition_rate <- round(
  employees_left / total_employees * 100,
  2
)

total_employees
employees_left
attrition_rate
```
<img width="474" height="344" alt="Screenshot 2026-05-25 135554" src="https://github.com/user-attachments/assets/97ad8c96-376c-435b-ac3c-7c2f27e0c89a" />
### Findings

The cleaned dataset contains 11,991 employees. A total of 1,991 employees left the company, resulting in an attrition rate of 16.60%.
## 4.2 Attrition by Department

This analysis examines which departments have higher employee turnover.
```r
# Calculate attrition rate by department
department_attrition <- hr_cleaned %>%
  group_by(department) %>%
  summarise(
    Total_Employees = n(),
    Employees_Left = sum(left),
    Attrition_Rate = round(Employees_Left / Total_Employees * 100, 2)
  ) %>%
  arrange(desc(Attrition_Rate))

department_attrition
```
<img width="629" height="311" alt="Screenshot 2026-05-25 135653" src="https://github.com/user-attachments/assets/ecb429bd-dfdd-4b8c-89bb-2dd0624ad4dc" />

### Findings

Attrition rates vary across departments. HR, Accounting, and Technical show some of the highest attrition rates, while Management has one of the lowest.
## 4.3 Satisfaction Level vs Attrition

This analysis compares satisfaction levels between employees who stayed and employees who left.
```r
# Average satisfaction by attrition status
satisfaction_analysis <- hr_cleaned %>%
  group_by(attrition_status) %>%
  summarise(
    Average_Satisfaction = round(mean(satisfaction_level), 2)
  )

satisfaction_analysis
```
<img width="654" height="272" alt="Screenshot 2026-05-25 135835" src="https://github.com/user-attachments/assets/8bb1a6a5-909a-4cc9-a408-6da67594ec1d" />

### Findings

Employees who left had a lower average satisfaction level than employees who stayed, suggesting that satisfaction is strongly related to retention.
## 4.4 Salary Level vs Attrition

This analysis examines attrition rates across salary levels.
```r
# Calculate attrition rate by salary level
salary_attrition <- hr_cleaned %>%
  group_by(salary) %>%
  summarise(
    Total_Employees = n(),
    Employees_Left = sum(left),
    Attrition_Rate = round(Employees_Left / Total_Employees * 100, 2)
  ) %>%
  arrange(desc(Attrition_Rate))

salary_attrition
```
<img width="720" height="353" alt="Screenshot 2026-05-25 135937" src="https://github.com/user-attachments/assets/ee8f95b2-149f-494a-acf2-e388de006bba" />
## 4.5 Workload Analysis

This analysis compares workload patterns between employees who stayed and employees who left.
```r
# Compare workload by attrition status
workload_analysis <- hr_cleaned %>%
  group_by(attrition_status) %>%
  summarise(
    Average_Projects = round(mean(number_project), 2),
    Average_Monthly_Hours = round(mean(average_montly_hours), 2)
  )

workload_analysis
```
<img width="689" height="300" alt="Screenshot 2026-05-25 140014" src="https://github.com/user-attachments/assets/bd22d579-d580-4ce0-a005-df035962896b" />
## 4.6 Promotion Impact

This analysis examines whether promotion history is related to employee attrition.
```r
# Calculate attrition rate by promotion status
promotion_attrition <- hr_cleaned %>%
  group_by(promotion_status) %>%
  summarise(
    Total_Employees = n(),
    Employees_Left = sum(left),
    Attrition_Rate = round(Employees_Left / Total_Employees * 100, 2)
  ) %>%
  arrange(desc(Attrition_Rate))

promotion_attrition
```
<img width="758" height="332" alt="Screenshot 2026-05-25 140109" src="https://github.com/user-attachments/assets/d4b9689b-25dd-44ab-be27-87e366f32353" />
### Findings

Employees who received promotions had a much lower attrition rate than employees who did not receive promotions, suggesting that career growth opportunities may support retention.

# 5. Share

## Dashboard Overview

After completing the exploratory data analysis, the cleaned dataset was imported into Tableau to create an interactive dashboard.

The purpose of the dashboard is to provide a visual summary of employee attrition patterns and highlight key factors associated with workforce retention.

The dashboard allows users to explore employee turnover trends across departments, salary levels, promotion status, and employee satisfaction.
```r
# Export cleaned dataset for Tableau

write_csv(
  hr_cleaned,
  "hr_cleaned_tableau.csv"
)
```
## Tableau Dashboard Sheets

### Sheet 1: Attrition Rate by Department

<img width="1040" height="635" alt="Sheet 1" src="https://github.com/user-attachments/assets/70879497-80e6-423e-b4e1-fd1b1921e2f2" />

---

### Sheet 2: Attrition Rate by Salary Level

<img width="328" height="670" alt="Sheet 2" src="https://github.com/user-attachments/assets/cc0f7830-4795-4399-8af0-35e2da4e96a7" />

---

### Sheet 3: Average Satisfaction by Attrition Status

<img width="253" height="705" alt="Sheet 3" src="https://github.com/user-attachments/assets/c903f94f-6405-4433-a65c-bcf1484d4c97" />

---

### Sheet 4: Promotion Status and Attrition

<img width="465" height="190" alt="Sheet 4" src="https://github.com/user-attachments/assets/65d781b4-ea8d-4d53-96a7-3e889143cd1c" />

---

### Sheet 5: Attrition Rate by Years at Company

<img width="1393" height="635" alt="Sheet 5 (1)" src="https://github.com/user-attachments/assets/7615687e-b2f4-4a7c-a2a8-62c1ad1ef370" />

---

### Sheet 6: Employee Distribution by Department

<img width="1125" height="634" alt="Sheet 6" src="https://github.com/user-attachments/assets/81d91ad5-737a-40d8-bd7b-98fc1759d7c2" />

# Dashboard Findings

## Attrition by Department

The analysis shows that attrition rates vary across departments. HR and technical-related departments experience higher turnover compared to other business functions, suggesting potential challenges in workload management, employee engagement, or career development opportunities.

---

## Attrition by Salary Level

Employees in the low salary category exhibit the highest attrition rate, while employees receiving high salaries show significantly lower turnover rates. This suggests that compensation may play an important role in employee retention.

---

## Employee Satisfaction and Attrition

Employees who left the organization reported substantially lower satisfaction levels than employees who stayed. This finding indicates a strong relationship between employee satisfaction and workforce retention.

---

## Promotion Impact

Employees who received promotions demonstrated lower attrition rates than employees who did not receive promotions. Career advancement opportunities appear to be an important factor influencing employee retention.

---

## Attrition by Years at Company

Employee turnover patterns vary across tenure levels. Certain years of service show elevated attrition rates, indicating critical periods where retention strategies may be most needed.

---

## Workforce Distribution by Department

The workforce is concentrated within a few major departments, particularly Sales and Technical. These departments represent a significant portion of the workforce and may require additional retention-focused initiatives due to their organizational importance.
# 6. Act

## Recommendations

Based on the analysis, several recommendations can be proposed to improve employee retention and workforce stability.

### 1. Improve Employee Satisfaction

Since employees who left reported significantly lower satisfaction levels, management should regularly monitor employee engagement and satisfaction through surveys, feedback sessions, and workplace improvement initiatives.

### 2. Review Compensation Strategies

The analysis indicates that employees in lower salary groups are more likely to leave. Reviewing compensation structures and ensuring competitive pay may help reduce turnover.

### 3. Increase Career Development Opportunities

Employees who received promotions demonstrated lower attrition rates. Organizations should strengthen career development programs, internal mobility opportunities, and transparent promotion pathways.

### 4. Focus on High-Risk Departments

Departments with higher attrition rates should receive targeted retention initiatives, including workload assessments, leadership support, and employee development programs.

### 5. Monitor Employee Tenure Trends

Attrition patterns across years of service suggest that employees may be more likely to leave during specific stages of their careers. Retention efforts should focus on these critical periods.

---

## Conclusion

This HR Analytics project analyzed employee attrition using R for data cleaning and exploratory analysis and Tableau for interactive dashboard development.

The findings reveal that employee satisfaction, salary level, promotion opportunities, and departmental differences all contribute to workforce retention outcomes.

By leveraging data-driven insights, organizations can develop more effective retention strategies, improve employee engagement, and support long-term workforce stability.
