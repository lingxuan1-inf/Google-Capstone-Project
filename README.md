# Google-Capstone-Project
Google Capstone Project for Advanced Data Analytics: Building a model to predict whether an employee will leave the company

# Salifort Motors HR Analytics: Predicting Employee Turnover

## 📌 Executive Summary
This project analyzes HR data from Salifort Motors to identify the primary drivers of employee turnover and builds a predictive machine learning model to flag high-risk employees. By leveraging a **Random Forest classifier**, the HR department can proactively target retention efforts. Key findings reveal that turnover is largely driven by two extremes: overworked top-performers (6+ projects, >250 hours) and underutilized, disengaged employees.

---

## 🎯 Business Problem
Currently, there is a high rate of turnover among Salifort employees. This is an issue as high employee turnover is time-consuming and expensive due to the resources required to find, interview, and train new hires. The goal of this project is to analyze 15,000 (14,999 based on the actual dataset) employee records, identify factors contributing to turnover, and build a predictive model so HR can proactively deploy retention strategies.

## 📊 Data & Approach
* **Dataset:** 14,999 observations and 10 features (Satisfaction, Evaluation, Projects, Monthly Hours, Tenure, Work Accident, Promotion, Department, and Salary).
* **Outcome Variable:** `left` (Binary: 1 = Left, 0 = Stayed). Highly imbalanced (approx. 83% stayed, 17% left).
* **Data Dictionary:** see below

Variable  |Description |
-----|-----|
satisfaction_level|Employee-reported job satisfaction level [0&ndash;1]|
last_evaluation|Score of employee's last performance review [0&ndash;1]|
number_project|Number of projects employee contributes to|
average_monthly_hours|Average number of hours employee worked per month|
time_spend_company|How long the employee has been with the company (years)
Work_accident|Whether or not the employee experienced an accident while at work
**left**|Whether or not the employee left the company
promotion_last_5years|Whether or not the employee was promoted in the last 5 years
Department|The employee's department
salary|The employee's salary (U.S. dollars)

* **Metrics Strategy:** The **F1-Score** was selected as the primary evaluation metric to balance the business cost of False Positives (wasting retention resources) and False Negatives (failing to identify flight-risk employees). 
* **Methodology:** Google's PACE (Plan, Analyze, Construct, Execute) framework.

## 🛠️ Process & Actions
1. **Data Cleaning:** Handled missing values, standardized column names to `snake_case`, and removed ~3,000 duplicated rows to prevent data leakage during cross-validation. 
2. **Exploratory Data Analysis (EDA):** Visualized distributions using KDE plots, boxplots, and correlation heatmaps to understand feature relationships with turnover. Analyze the relationship between outcome variable and feature variable as well deeper analysis of variable features.
3. **Feature Engineering:** * `overworked`: Flagged employees working > 250 hours with >= 6 projects.
    * `tenure_risk`: Flagged employees in the high-risk tenure window (3 to 6 years).
4. **Data Preprocessing:** Utilized `ColumnTransformer` and `Pipeline` to apply `StandardScaler` to continuous variables, `OrdinalEncoder` to salary, and `OneHotEncoder` to nominal departments.
5. **Model Training:** Stratified 80/20 train-test split. Trained baseline and optimized models using 5-fold cross-validation (`StratifiedKFold`) and `GridSearchCV` for hyperparameter tuning.

## 💡 Key Insights
* **The Bimodal Exodus:** Employees are leaving for two completely different reasons. One group is extremely overworked (high hours, high evaluation, high project count) while the other is underutilized and disengaged (low hours, low evaluation).
* **The "Project Load" Sweet Spot:** Employees assigned exactly 3 projects have the highest retention and job satisfaction. Comparatively, 100% of employees assigned 7 projects left. The same pattern in working hours also exists in projects load where there are many who are underutilized or overworked. 
* **Career Stagnation:** Promotions are incredibly rare. In fact, the number of promoted employee who work long hours were very low.  However, the few employees who did receive a promotion in the last 5 years were 80% less likely to leave.
* **The High-Risk Window:** Turnover spikes heavily among employees who have been with the company for 3 to 6 years. 

## 🤖 Models Trained
* **Logistic Regression:** Tuned with custom decision thresholds (Threshold = 0.2) to prioritize recall, achieving an F1-Score of ~0.61.
* **Naive Bayes (Gaussian & Complement):** Provided a baseline with an F1-Score of ~0.62.
* **Decision Tree:** Explored to understand non-linear splits and feature importance.
* **Random Forest:** Explored as an alternative to DT given it ability to reduce variance via bagging
* **XGBoost:** Evaluated as a gradient boosting alternative.

### 🏆 Champion Model: Random Forest
The **Random Forest Classifier** outperformed all other models, effectively capturing the complex, non-linear relationships present in the data while providing robust resistance to overfitting. 

**Test Set Performance:**
* **Accuracy:** 98.58%
* **Precision:** 98.66%
* **Recall:** 92.71%
* **F1-Score:** 95.60%
* **ROC-AUC:** 97.71%

## 📈 Business Recommendations
1. **Cap Project Workloads:** While the data shows that retention rate peaks at number of projects, a more accurate representation will be the duration of projects. More data will be required if the HR department requires further investigation. However, we do recommend enforce a soft cap of 3 to 4 projects per employee in their first 3 years where tenure_risk is low. We could also mandate a review for any employee taking on 5+ projects to ensure monthly hours do not exceed 250.
2. **Revamp Career Progression:** Implement structured career pathways and clear promotion criteria, especially for employees hitting their 3-year tenure mark, as lack of promotion is a massive turnover driver.
3. **Investigate Task Delegation:** Conduct a managerial audit. The data shows systemic inequality in work distribution, causing top-performers to burn out and underutilized employees to disengage.
4. **Targeted Retention:** Utilize the **Random Forest** model to flag current employees fitting high-risk profiles (e.g., tenure of 3-6 years, high project load). Employees who are flagged will required one-on-one  interviews to understand more of thier concerns and workload rebalancing.
5. **Perform Qualitative Surveys (Advanced):** Utilizing response from employees, we can perform NLP to extract top 20 -30 common word, or n-gram phrases to find common reasons for staying/leaving the company. This could potentially improve the accuracy of the model. 

## ℹ️ More details
For more details, do refer to the capstone_projects notebook for more information on the EDA and ML process for this project.  

## Thank you for reading my project!!