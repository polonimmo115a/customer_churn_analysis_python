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

**Observations:** The majority of customers who do not churn tend to have services like PhoneService, InternetService (particularly DSL), and OnlineSecurity enabled. For services like OnlineBackup, TechSupport, and StreamingTV, churn rates are noticeably higher when these services are not used or are unavailable.

### Churn Analysis by Contract Type

```python

plt.figure(figsize = (4,4))
ax=sns.countplot(x="Contract",data=df,hue="Churn")
ax.bar_label(ax.containers[0])  
plt.title("Count of Customers by Contract")
plt.savefig('Count_of_customers_by_contract.png', dpi=300, bbox_inches='tight')
plt.show()
```

![Count_of_customers_by_contract](https://github.com/user-attachments/assets/3bce1ec8-97f4-4a2b-8f86-f112c8af12f5)

**Observations:** people who have month to month contract are likely to churn than from those who have 1 or 2 years or contract.

### Analysis of churn through hisplot based on tenure

```python

plt.figure(figsize = (9,4))
sns.histplot(x='tenure', data=df,bins=72,hue="Churn")
plt.savefig('histplot_of_tenure.png', dpi=300, bbox_inches='tight')
plt.show()
```

![Histogram of Tenure](https://github.com/user-attachments/assets/deee59aa-cfe6-470d-b58a-39fa2b5e9262)

**Observations:** people who have used our service for long time stayed,but the people who have used the service for 1 or 2 months have churned

### Analysis of Churn By Senior Citizen

```python

total_counts = df.groupby('SeniorCitizen')['Churn'].value_counts(normalize=True).unstack() * 100

# Plot
fig, ax = plt.subplots(figsize=(4, 4))  # Adjust figsize for better visualization

# Plot the bars
total_counts.plot(kind='bar', stacked=True, ax=ax, color=['#1f77b4', '#ff7f0e'])  # Customize colors if desired

# Add percentage labels on the bars
for p in ax.patches:
    width, height = p.get_width(), p.get_height()
    x, y = p.get_xy()
    ax.text(x + width / 2, y + height / 2, f'{height:.1f}%', ha='center', va='center')

plt.title('Churn by Senior Citizen (Stacked Bar Chart)')
plt.xlabel('SeniorCitizen')
plt.ylabel('Percentage (%)')
plt.xticks(rotation=0)
plt.legend(title='Churn', bbox_to_anchor = (0.9,0.9))  # Customize legend location
plt.savefig('churn_by_senior_citizen_stackedbarchart.png', dpi=300, bbox_inches='tight')

plt.show()
```

![Churn by Senior Citizen Stacked Bar Chart](https://github.com/user-attachments/assets/8e782c46-b7fc-4654-8689-54f40d2c24d4)

**Observations:** comparitively a greater percentage of people in senior citizen category have churned

### Analysis of Customer Churn by percentage

```python

plt.figure(figsize = (3,4))
plt.title("Percentage of Churned Customeres", fontsize = 10)
plt.pie(gb["Churn"], labels=gb.index, autopct="%1.2f%%")
plt.savefig('percentage_of_churned_customers.png', dpi=300, bbox_inches='tight')
plt.show()
```

![Percentage of Churned Customers](https://github.com/user-attachments/assets/587b2202-d0f2-4aa7-9eda-1c292016e48b)

**Observations:** from the given pie chart we conclude that 26.54% customers have churned


## Final Business Insights

* The overall customer churn rate was 26.5%, meaning nearly 1 out of every 4 customers discontinued telecom services, creating significant recurring revenue loss.
* Customers with Month-to-Month contracts contributed more than 85% of total churn, making short-term contracts the strongest churn indicator.
* Churned customers had an average monthly charge of ₹74.4, compared to ₹61.3 for retained customers, indicating that higher pricing directly increased churn probability.
* Nearly 48% of churned customers were high-value premium users with monthly charges above ₹80, resulting in major revenue leakage from profitable customer segments.
* Customers using Fiber Optic internet services accounted for over 80% of churn-related revenue loss, highlighting service quality and pricing concerns among premium subscribers.
* New customers with tenure below 12 months showed the highest churn behavior, indicating weak onboarding and early-stage customer engagement.

## Strategic Recommendations

* Introduce discounts and loyalty benefits for customers upgrading to long-term contracts.
* Launch targeted retention campaigns for high-value Fiber Optic customers to reduce premium customer churn.
* Improve onboarding experience and proactive engagement during the first 90 days of customer lifecycle.
* Bundle Tech Support, Online Security, and Device Protection services to improve customer retention and perceived service value.
* Implement predictive churn monitoring for high-bill customers to reduce future revenue leakage and improve customer lifetime value (CLV










