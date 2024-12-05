# 📊 **Global Superstore Data Analysis** 🛍️
This project focuses on analyzing the **Global Superstore dataset** using Python. The analysis aims to uncover insights related to sales, profit, discounts, and regional performance through descriptive statistics, correlations, and a variety of visualizations.

  ![image](https://github.com/user-attachments/assets/c7f53389-5541-4c86-9605-69ddee6039b1)

---
## 🎯 **Objective**
1. Understand trends in sales, profit, and discounts.
2. Identify relationships between variables using statistical analysis.
3. Provide actionable insights to optimize profitability and performance.
---
## 🛠️ **Key Technologies Used**
- **Programming Language**: Python
- **Libraries**:
  - `pandas`: Data cleaning and manipulation
  - `numpy`: Numerical computations
  - `matplotlib` & `seaborn`: Data visualization
  - `scipy`: Statistical analysis
---
# 📊 Global Superstore Data Analysis
---
##  Load the Dataset
```python
df = pd.read_excel("Global_Superstore2.xlsx")
```---
##  Explore the Dataset
```python
print("First 5 rows of the dataset:")
print(df.head())

print("\nDataset Info:")
print(df.info())
```
###  Data Cleaning
```python
#Fill missing numeric values with the mean
df.fillna(df.mean(numeric_only=True), inplace=True)
```
###  Feature Engineering
```python
# Create new features: Log-transformed sales and Profit Margin
df["Log_Sales"] = np.log1p(df["Sales"])  
df["Profit_Margin"] = df["Profit"] / df["Sales"]  
```python
###  Descriptive statistics for numeric columns
```python
print("\nDescriptive Statistics:")
print(df.describe())
```
### Correlation Analysis
```python
# Calculate the correlation matrix
correlation_matrix = df[["Sales", "Profit", "Quantity", "Discount", "Profit_Margin"]].corr()
print("\nCorrelation Matrix:")
print(correlation_matrix)
```
### Heatmap
**Heatmap (Correlation Matrix)**:
   - Displays relationships between numeric variables like Sales, Profit, and Discount.
   - Helps identify strongly correlated variables. 
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Plot a heatmap of correlations
plt.figure(figsize=(10, 6))
sns.heatmap(correlation_matrix, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Heatmap (Seaborn)")
plt.show()
```
   ![image](https://github.com/user-attachments/assets/18f6e622-bd4f-43d5-a927-b077b3cbc216)   
### Line Plot
**Line Plot (Monthly Sales Trend)**:
   - Reveals seasonal trends in sales over time.
   - Highlights peaks and slow periods.
```python
# Plot monthly sales over time
df["Order Date"] = pd.to_datetime(df["Order Date"])  # Ensure Order Date is in datetime format
monthly_sales = df.groupby(pd.Grouper(key="Order Date", freq="M"))["Sales"].sum()

plt.figure(figsize=(10, 6))
plt.plot(monthly_sales.index, monthly_sales.values, color="blue", label="Monthly Sales")
plt.title("Monthly Sales Over Time (Matplotlib)")
plt.xlabel("Date")
plt.ylabel("Total Sales")
plt.legend()
plt.grid()
plt.show()
```
   ![image](https://github.com/user-attachments/assets/794f6a46-f111-4675-861f-c78396bd7ce3)
### Boxplot of Profit Margins by Region
   - Compares variability and performance across different regions. 
```python
# Boxplot for Profit Margins by Region
if "Region" in df.columns:
    plt.figure(figsize=(8, 5))
    sns.boxplot(data=df, x="Region", y="Profit_Margin", palette="Set2")
    plt.title("Profit Margins by Region (Seaborn)")
    plt.xlabel("Region")
    plt.ylabel("Profit Margin")
    plt.show()
```
   ![image](https://github.com/user-attachments/assets/269be25e-f617-40c7-91f4-3ab123f5fe72)
### Scatter Plot of Sales vs Profit (with trendline)
   - Visualizes the relationship between sales and profit.
   - Includes a trendline with R² to show correlation strength.
``` python
if "Sales" in df.columns and "Profit" in df.columns:
    plt.figure(figsize=(10, 6))
    sns.scatterplot(data=df, x="Sales", y="Profit", hue="Segment", palette="viridis")
    # Add trendline
    slope, intercept, r_value, _, _ = linregress(df["Sales"], df["Profit"])
    plt.plot(df["Sales"], slope * df["Sales"] + intercept, color="red", label=f"Trendline (R²={r_value**2:.2f})")
    plt.title("Sales vs Profit by Segment (Seaborn)")
    plt.xlabel("Sales")
    plt.ylabel("Profit")
    plt.legend()
    plt.grid()
    plt.show()
```
   ![image](https://github.com/user-attachments/assets/1e51ba2e-91bf-4101-894f-c5f9893d9fa9)
### Bar Chart of Average Sales by Category
   - Identifies product categories driving the most revenue.
``` python
if "Category" in df.columns:
    avg_sales_by_category = df.groupby("Category")["Sales"].mean()
    plt.figure(figsize=(8, 5))
    avg_sales_by_category.plot(kind="bar", color="skyblue")
    plt.title("Average Sales by Category (Matplotlib)")
    plt.xlabel("Category")
    plt.ylabel("Average Sales")
    plt.show()
```
  ![image](https://github.com/user-attachments/assets/87cb4c80-13ab-46f4-bd3a-f904f1be0bc1)
###  Histogram of Discount Distribution
   - Analyzes the frequency of different discount levels.   
```python
if "Discount" in df.columns:
    plt.figure(figsize=(8, 5))
    sns.histplot(df["Discount"], kde=True, bins=20, color="purple")
    plt.title("Distribution of Discounts (Seaborn)")
    plt.xlabel("Discount")
    plt.ylabel("Frequency")
    plt.show()
```
 ![image](https://github.com/user-attachments/assets/0246bd06-126d-41de-a02b-63e159d0076d)
### Violin Plot of Sales by Segment
   - Compares sales distributions across customer segments.
  
``` python
if "Segment" in df.columns:
    plt.figure(figsize=(8, 5))
    sns.violinplot(data=df, x="Segment", y="Sales", palette="muted")
    plt.title("Sales Distribution by Segment (Seaborn)")
    plt.xlabel("Segment")
    plt.ylabel("Sales")
    plt.show()
```
 ![image](https://github.com/user-attachments/assets/34cf7a7b-8f89-4f69-a1bd-e48e4a51950e) 
 ### Bar chart for the above analysis
Analyze Discounts Effect on Profits
```python
# Analyze the effect of discounts on profits
discount_bins = pd.cut(df["Discount"], bins=[0, 0.1, 0.2, 0.3, 0.4, 1.0], labels=["0-10%", "10-20%", "20-30%", "30-40%", "40%+"])
avg_profit_by_discount = df.groupby(discount_bins)["Profit"].mean()

print("\nAverage Profit by Discount Range:")
print(avg_profit_by_discount)

# Bar chart for the above analysis
avg_profit_by_discount.plot(kind="bar", color="coral", title="Average Profit by Discount Range")
plt.xlabel("Discount Range")
plt.ylabel("Average Profit")
plt.show()
```
  ![image](https://github.com/user-attachments/assets/bc50f292-0d1c-4c3a-aef9-d3b8a7fda87a)
---
## 🔍 **Insights and Findings**
1. **Sales and Profit Relationship**:
   - Positive correlation observed; higher sales often lead to higher profit.
2. **Regional Performance**:
   - Profit margins vary significantly by region.
3. **Discount Effects**:
   - Extreme discounts (over 30%) negatively impact profits.
4. **Category Analysis**:
   - Certain categories, such as "Technology," generate higher average sales.
5. **Monthly Trends**:
   - Clear seasonal patterns with peak sales during specific months.
   - ---
## 📌 **Recommendations**
1. **Optimize Discounts**:
   - Limit discounts above 30%, as they reduce profitability.
2. **Focus on High-Margin Regions**:
   - Prioritize regions with consistent profit margins for expansion.
3. **Invest in High-Performing Categories**:
   - Allocate resources to categories like "Technology" and "Furniture."
4. **Seasonal Promotions**:
   - Target high-sales months with strategic marketing campaigns.
---
## 📂 **Project Files**
- **Python Code**: Contains all data cleaning, feature engineering, and analysis steps.
- **Dataset**: Global Superstore data file used for the project.
- **Visualizations**: Key plots generated during the analysis.
---
If you find this project useful, feel free to ⭐ star the repository!
