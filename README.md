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
![average_monthly_charges_churnstatus](https://github.com/user-attachments/assets/9a389d8e-d0b3-414e-8651-249731d7e365)

**observation:** Customers with higher monthly charges were more likely to churn

### MONTHLY REVENUE LOSS DUE TO CHURN

```python

churned_customers = df[df["Churn"] == "Yes"]

revenue_lost = churned_customers["MonthlyCharges"].sum()
revenue_lost
```

### REVENUE LOST BY CONTRACT TYPE

```python

contract_revenue_loss = (
    churned_customers.groupby("Contract")
    ["MonthlyCharges"]
    .sum()
    .reset_index()
    .sort_values(by="MonthlyCharges",
                 ascending=False)
)
contract_revenue_loss
```

### Revenue loss by internet service

```python

internet_revenue_loss = (
    churned_customers.groupby("InternetService")
    ["MonthlyCharges"]
    .sum()
    .reset_index()
    .sort_values(by="MonthlyCharges",
                 ascending=False)
)
internet_revenue_loss
```

### Indentification of high value churn

```python

high_value_churned=churned_customers[churned_customers["MonthlyCharges"]>80]
high_value_churned.head()
```

## Business Insight

* Month-to-month customers contributed the highest revenue loss.

* Fiber optic users had the highest churn-related revenue leakage.

* High-paying customers showed disproportionately high churn rates.

* Long-term contract customers generated more stable revenue streams


## Some more Business Observation

### Churned Customer By Payment Method

```python

plt.figure(figsize = (8,4))
ax=sns.countplot(x="PaymentMethod",data=df,hue="Churn")
ax.bar_label(ax.containers[0])
ax.bar_label(ax.containers[1])
plt.xticks(rotation=45)
plt.title("Churned Customers by Payment method")
plt.savefig('churned_customers_by_paymentmethod.png', dpi=300, bbox_inches='tight')
plt.show()
```

![churned_customers_by_paymentmethod](https://github.com/user-attachments/assets/f2e17abc-764a-4488-b7b3-612478352592)

**Observations:** customer is likely to churn when he is using electronic check as a payment method.

### Churn analysis of customers by phoneservice,Multiplelines,Internet Service,online Security,Online Backup,Device Protection,Techsupport,Streaming TV,Streaming
movies

```python

columns = ['PhoneService', 'MultipleLines', 'InternetService', 'OnlineSecurity', 
           'OnlineBackup', 'DeviceProtection', 'TechSupport', 'StreamingTV', 'StreamingMovies']

# Number of columns for the subplot grid (you can change this)
n_cols = 3
n_rows = (len(columns) + n_cols - 1) // n_cols  # Calculate number of rows needed

# Create subplots
fig, axes = plt.subplots(n_rows, n_cols, figsize=(15, n_rows * 4))  # Adjust figsize as needed

# Flatten the axes array for easy iteration (handles both 1D and 2D arrays)
axes = axes.flatten()

# Iterate over columns and plot count plots
for i, col in enumerate(columns):
    sns.countplot(x=col, data=df, ax=axes[i], hue = df["Churn"])
    axes[i].set_title(f'Count Plot of {col}')
    axes[i].set_xlabel(col)
    axes[i].set_ylabel('Count')

# Remove empty subplots (if any)
for j in range(i + 1, len(axes)):
    fig.delaxes(axes[j])

plt.tight_layout()
plt.savefig('subplot.png', dpi=300, bbox_inches='tight')
plt.show()

```

![subplot](https://github.com/user-attachments/assets/ea0539fd-6528-4f68-8292-a669c801f9fc)

