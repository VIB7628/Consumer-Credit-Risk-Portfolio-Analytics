# Consumer Credit Risk & Portfolio Analytics
## 📌 Project Overview
As an Electronics and Computer Engineering student, I developed this project to demonstrate the intersection of data engineering and business intelligence. This project analyzes consumer credit data to identify risk patterns, monitor portfolio health, and simulate financial scenarios.
## 🛠️ Technical Stack
- **Data Cleaning:** Python (Pandas, NumPy)
- **Visualization:** Power BI Desktop
- **Language:** DAX (Data Analysis Expressions) & SQL
- **Environment:** Google Colab
## 🚀 The Workflow
### 1. Data Cleaning (Python/Pandas)
Raw credit data is often messy. I used Python to:
- **Scale Anomalies:** Fixed credit scores that were erroneously scaled (e.g., scores like 7000 normalized to 700).
- **Imputation:** Handled missing values for 'Annual Income' using median values to avoid skewing results.
- **Feature Engineering:** Created the `Utilization %` metric and a `Public Record Flag` to categorize borrower risk levels.
### 2. Business Intelligence (Power BI)
I transformed the cleaned CSV into an interactive dashboard featuring:
- **Risk Gauge:** Monitoring credit utilization against a 30% industry benchmark.
- **What-If Analysis:** A parameter-driven slider to simulate interest rate impacts on total debt.
- **Smart Narrative:** AI-generated summaries that explain data trends in plain English.
---
## 📊 SQL Knowledge (Extension)
To ensure the project is scalable, I drafted SQL queries to handle the same analysis in a relational database environment.
### Sample Questions & Solutions
---
### **SQL Portfolio Extension: Analysis & Querying**

---

#### **Level: Easy (Data Retrieval & Filtering)**

**1. Find all customers with a Credit Score above 700.**
```sql
SELECT Customer_ID, Credit_Score 
FROM Credit_Portfolio 
WHERE Credit_Score > 700;
```

---

**2. List the unique purposes for which people are taking loans.**
```sql
SELECT DISTINCT Loan_Purpose 
FROM Credit_Portfolio;
```

---

**3. Count how many people have a "Short Term" loan.**
```sql
SELECT COUNT(*) AS Total_Short_Term_Loans
FROM Credit_Portfolio 
WHERE Term = 'Short Term';
```

---

**4. Find borrowers with a "Home Mortgage" and an "Annual Income" > 100,000.**
```sql
SELECT * FROM Credit_Portfolio 
WHERE Home_Ownership = 'Home Mortgage' 
AND Annual_Income > 100000;
```

---

**5. Show the top 5 customers with the highest "Monthly Debt."**
```sql
SELECT Customer_ID, Monthly_Debt 
FROM Credit_Portfolio 
ORDER BY Monthly_Debt DESC 
LIMIT 5;
```

---

#### **Level: Medium (Aggregation & Logic)**

**6. Average Credit Score for each "Home Ownership" category.**
```sql
SELECT Home_Ownership, AVG(Credit_Score) AS Avg_Credit_Score
FROM Credit_Portfolio
GROUP BY Home_Ownership;
```

---

**7. Total Loan Amount issued for each "Loan Purpose."**
```sql
SELECT Loan_Purpose, SUM(Current_Loan_Amount) AS Total_Loaned
FROM Credit_Portfolio
GROUP BY Loan_Purpose;
```

---

**8. Find the percentage of customers who have "Tax Liens."**
```sql
SELECT (COUNT(CASE WHEN Tax_Liens > 0 THEN 1 END) * 100.0 / COUNT(*)) AS Lien_Percentage
FROM Credit_Portfolio;
```

---

**9. List "Years in current job" where average income is over 50,000.**
```sql
SELECT Years_in_current_job, AVG(Annual_Income) AS Avg_Income
FROM Credit_Portfolio
GROUP BY Years_in_current_job
HAVING AVG(Annual_Income) > 50000;
```

---

**10. Categorize borrowers by "Risk_Level" based on Credit Score.**
```sql
SELECT Customer_ID, Credit_Score,
CASE 
    WHEN Credit_Score > 750 THEN 'Excellent'
    WHEN Credit_Score > 650 THEN 'Good'
    ELSE 'Poor'
END AS Risk_Level
FROM Credit_Portfolio;
```

---

#### **Level: Hard (Advanced Analytics)**

**11. Find "Credit Utilization Rate" (Balance/Limit) above 80%.**
```sql
SELECT Customer_ID, (Current_Credit_Balance / Maximum_Open_Credit) AS Utilization
FROM Credit_Portfolio
WHERE Maximum_Open_Credit > 0 
AND (Current_Credit_Balance / Maximum_Open_Credit) > 0.8;
```

---

**12. Identify "High Risk Outliers" (Debt > 50% of Monthly Income).**
```sql
SELECT Customer_ID, Annual_Income, Monthly_Debt
FROM Credit_Portfolio
WHERE Monthly_Debt > (Annual_Income / 12 * 0.5);
```

---

**13. Rank customers within each "Loan Purpose" by Credit Score.**
```sql
SELECT Customer_ID, Loan_Purpose, Credit_Score,
RANK() OVER(PARTITION BY Loan_Purpose ORDER BY Credit_Score DESC) AS Rank_In_Category
FROM Credit_Portfolio;
```

---

**14. Calculate a Running Total of Loan Amounts issued over time.**
```sql
SELECT Issue_Date, Current_Loan_Amount,
SUM(Current_Loan_Amount) OVER(ORDER BY Issue_Date) AS Running_Total
FROM Credit_Portfolio;
```

---

**15. Find pairs of customers with same "Annual Income" but different "Credit Scores."**
```sql
SELECT a.Customer_ID, b.Customer_ID, a.Annual_Income
FROM Credit_Portfolio a
JOIN Credit_Portfolio b ON a.Annual_Income = b.Annual_Income
WHERE a.Customer_ID <> b.Customer_ID 
AND a.Credit_Score > b.Credit_Score;
