# Power BI – Power Query Transformations & DAX Measures

## Power Query Transformations

### 1. Add New Columns in `prod_Churn`

```
Churn Status = if [Customer_Status] = "Churned" then 1 else 0
```
> Change `Churn Status` data type to **Number**.

```
Monthly Charge Range = if [Monthly_Charge] < 20 then "< 20"
    else if [Monthly_Charge] < 50 then "20-50"
    else if [Monthly_Charge] < 100 then "50-100"
    else "> 100"
```

---

### 2. New Reference Table – `mapping_AgeGrp`

- Keep only the `Age` column and remove duplicates.

```
Age Group = if [Age] < 20 then "< 20"
    else if [Age] < 36 then "20 - 35"
    else if [Age] < 51 then "36 - 50"
    else "> 50"
```

```
AgeGrpSorting = if [Age Group] = "< 20" then 1
    else if [Age Group] = "20 - 35" then 2
    else if [Age Group] = "36 - 50" then 3
    else 4
```
> Change `AgeGrpSorting` data type to **Number**.

---

### 3. New Reference Table – `mapping_TenureGrp`

- Keep only `Tenure_in_Months` and remove duplicates.

```
Tenure Group = if [Tenure_in_Months] < 6 then "< 6 Months"
    else if [Tenure_in_Months] < 12 then "6-12 Months"
    else if [Tenure_in_Months] < 18 then "12-18 Months"
    else if [Tenure_in_Months] < 24 then "18-24 Months"
    else ">= 24 Months"
```

```
TenureGrpSorting = if [Tenure_in_Months] = "< 6 Months" then 1
    else if [Tenure_in_Months] = "6-12 Months" then 2
    else if [Tenure_in_Months] = "12-18 Months" then 3
    else if [Tenure_in_Months] = "18-24 Months" then 4
    else 5
```
> Change `TenureGrpSorting` data type to **Number**.

---

### 4. New Reference Table – `prod_Services`

- Unpivot all service columns.
- Rename columns:
  - `Attribute` → `Services`
  - `Value` → `Status`

---

## DAX Measures

### Summary Page

```dax
Total Customers = COUNT(prod_Churn[Customer_ID])

New Joiners = CALCULATE(COUNT(prod_Churn[Customer_ID]), prod_Churn[Customer_Status] = "Joined")

Total Churn = SUM(prod_Churn[Churn Status])

Churn Rate = [Total Churn] / [Total Customers]
```

### Churn Prediction Page

```dax
Count Predicted Churner = COUNT(Predictions[Customer_ID]) + 0

Title Predicted Churners = "COUNT OF PREDICTED CHURNERS : " & COUNT(Predictions[Customer_ID])
```
