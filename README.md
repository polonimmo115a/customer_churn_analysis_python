# customer_churn_analysis_python
Telecom churn analytics project analyzing customer behavior, contract types, service usage, and revenue loss to provide actionable retention insights using Python.

# Overview of the project

This project focuses on analyzing customer churn behavior in the telecom industry using Python-based Exploratory Data Analysis (EDA). The objective of this analysis is to identify the major factors contributing to customer attrition, understand customer behavior patterns, and provide data-driven business recommendations to improve customer retention and reduce revenue loss.

The analysis revealed that customers with month-to-month contracts, higher monthly charges, shorter tenure, and limited support services showed significantly higher churn rates. Additionally, premium fiber-optic customers contributed the highest revenue leakage, highlighting the need for targeted retention strategies.

The project is designed to simulate a real-world telecom business problem and showcase practical analytical thinking, stakeholder-focused reporting, and business-driven decision-making skills

## Business Problem

### Average Monthly Charges by Churn Status

```python

monthly_charge_analysis = (
    df.groupby("Churn")["MonthlyCharges"]
    .mean()
    .reset_index()
)
monthly_charge_analysis
```

```python

ax=sns.barplot(
    data=monthly_charge_analysis, x="Churn", y="MonthlyCharges", hue="Churn"
)
for container in ax.containers:
    ax.bar_label(container, fmt="%.2f", padding=3)
plt.title("Average Monthly Charges by Churn Status")
plt.xlabel("Churn Status")
plt.ylabel("Mean Monthly Charges")
plt.savefig('average_monthly_charges_churnstatus.png', dpi=300, bbox_inches='tight')
plt.show()
```
!


