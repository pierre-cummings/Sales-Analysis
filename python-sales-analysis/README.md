# Python Sales Analysis

## Project Overview

This project uses Python to perform exploratory data analysis (EDA) on a retail sales dataset. The goal is to uncover insights related to revenue trends, product performance, and regional sales distribution.

The analysis demonstrates how Python can be used to clean data, perform aggregations, and generate visualizations that support business decision-making.

---

## Tools Used

- Python
- Pandas
- Matplotlib
- Jupyter Notebook

---

## Project Structure

```
python-sales-analysis
│
├── Python_EDA
│   ├── eda_analysis.ipynb
│   └── superstore.csv
│
├── charts
│   ├── category_sales.png
│   ├── monthly_sales.png
│   ├── region_sales.png
│   └── top_products.png
│
└── README.md
```

---

## Data Preparation

The dataset was loaded into a pandas DataFrame and prepared for analysis by:

- Converting `Order_Date` to datetime format
- Verifying column structure and data types
- Creating a `Month` column for time-based analysis

---

## Key Analysis

### 1. Sales by Region

#### Business Question
Which regions generate the most revenue?

#### Code

```python
region_sales = df.groupby('Region')['Sales'].sum()
```

#### Why This Matters to the Business

Understanding regional performance helps businesses allocate resources, optimize logistics, and target high-performing markets.

## Result

<img width="1169" height="773" alt="region_sales" src="https://github.com/user-attachments/assets/1efb9865-7c5f-450e-acde-7918578af013" />


---

### 2. Monthly Sales Trend

#### Business Question
How does revenue change over time?

#### Code

```python
df['Order Date'] = pd.to_datetime(df['Order Date'])
df['Ship Date'] = pd.to_datetime(df['Ship Date'])
monthly_sales = df.groupby(df['Order Date'].dt.to_period('M'))['Sales'].sum()
```

#### Why This Matters to the Business

Analyzing trends over time helps identify seasonality, forecast demand, and plan promotions.

#### Result

<img width="1159" height="853" alt="monthly_sales" src="https://github.com/user-attachments/assets/b99b5ce8-c8e2-4d58-a1bd-191e9707a1a9" />


---

### 3. Sales by Category

#### Business Question
Which product categories generate the most revenue?

#### Code

```python
category_sales = df.groupby('Category')['Sales'].sum()
```

#### Why This Matters to the Business

This helps businesses understand which product groups drive revenue and where to focus inventory and marketing efforts.

#### Result

<img width="1159" height="845" alt="category_sales" src="https://github.com/user-attachments/assets/d4f056cd-f3a5-433c-810f-e8f79f4108ec" />


---

### 4. Top Products by Sales

#### Business Question
Which products generate the highest revenue?

#### Code

```python
top_products = df.groupby('Product Name')['Sales'].sum().sort_values(ascending=False).head(10)
```

#### Why This Matters to the Business

Identifying top-performing products helps guide pricing, promotions, and stocking strategies.

#### Result

<img width="1153" height="871" alt="top_products" src="https://github.com/user-attachments/assets/edd91a47-0387-4f20-92ce-262f43c84bde" />


---

## Key Insights

- Sales performance varies significantly across regions
- A small number of products generate a large share of total revenue
- Category performance shows clear differences in revenue contribution
- Monthly sales trends indicate potential seasonality

---

## Skills Demonstrated

- Data Cleaning (pandas)
- Data Aggregation
- Exploratory Data Analysis (EDA)
- Data Visualization (matplotlib)
- Business Insight Generation
