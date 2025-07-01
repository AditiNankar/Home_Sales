# 🏠 Home Sales Data Analysis with PySpark SQL

## 📌 Overview

This project explores a large dataset of real estate home sales using **Apache SparkSQL**. The goal is to derive meaningful insights from the dataset, such as average sale prices by home attributes and year, and demonstrate the performance impact of using **temporary views**, **caching**, and **partitioning** in Spark.

This work is part of a larger exercise in big data analysis and optimization using PySpark.

---

## 📁 Dataset

The dataset used for this challenge is `home_sales_revised.csv`, located in an AWS S3 bucket. It contains information on:
- Sale price
- Number of bedrooms and bathrooms
- Square footage
- Number of floors
- View rating
- Date sold and date built

---

## 🧰 Tools & Technologies

- Python 3
- PySpark (SparkSQL)
- AWS S3 (data source)
- Jupyter Notebook / Google Colab

---

## ⚙️ Setup & Execution

### ✅ Initial Setup
- Imported necessary libraries from `pyspark.sql` and `pyspark.sql.functions`
- Loaded the CSV file using `spark.read.csv(...)` with headers and infer schema
- Created a temporary view of the DataFrame: `home_sales`

---

## 📊 Analysis & Queries

### 1. Average Price for 4-Bedroom Homes Sold Each Year

SELECT YEAR(date_sold) AS year_sold, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
WHERE bedrooms = 4
GROUP BY year_sold
ORDER BY year_sold;

### 2. Average Price for 3-Bedroom, 3-Bath Homes by Year Built

SELECT date_built, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
WHERE bedrooms = 3 AND bathrooms = 3
GROUP BY date_built
ORDER BY date_built;

### 3. Average Price for 3-Bed, 3-Bath, 2-Floor Homes ≥ 2,000 SqFt by Year Built

SELECT date_built, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
WHERE bedrooms = 3 AND bathrooms = 3 AND floors = 2 AND sqft_living >= 2000
GROUP BY date_built
ORDER BY date_built;

### 4. Average Price by View Rating (Where Avg ≥ $350,000)

SELECT view, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
GROUP BY view
HAVING AVG(price) >= 350000
ORDER BY view;

Runtime:
	•	Uncached: ~3.2 seconds
	•	Insight: Homes with better views tend to be priced significantly higher.

 ---

### 🚀 Performance Optimization

Step 1: Caching the Table
spark.catalog.cacheTable("home_sales")
spark.catalog.isCached("home_sales")  # ✅ Returns True

Step 2: Partitioning Data by date_built
home_sales_df.write.partitionBy("date_built").mode("overwrite").parquet("home_sales_partitioned")

---

### ✅ Summary & Insights
	•	SparkSQL allows complex SQL-like querying on distributed data efficiently.
	•	Caching improves query performance significantly for repeated operations.
	•	Partitioning + Parquet storage leads to the best performance in I/O-intensive operations.
	•	Structured queries on housing features (e.g. views, size, build year) can help model pricing trends and support real estate investment decisions.

---

## 👤 Author

Aditi Nankar
