````md
# 🍛 Databricks Notebook — Small Restaurant Ingestion & Analysis
**Files:** `zomato.csv`, `Country-Code.xlsx` (Sheet1), `file1.json…file5.json`

> Copy each cell into a Databricks notebook **as-is**. Cells are labeled with `%md`, `%python`, or `%sql`.

---

%md
# 1) Prerequisites & What You’ll Do
- Locate your files in **DBFS** (Databricks File System)
- Ingest: CSV (Zomato), Excel (Country codes), JSON (orders)
- Persist to **Delta** tables in a new database `restaurant_demo`
- Create views (Silver) and run example **SQL** analyses

> **Tip:** In Community Edition, reading Excel via Spark connector may be limited. This notebook uses a **pandas → Spark** fallback that works on most clusters.

---

%python
# 2) Set your paths here (EDIT base_dir if needed)
base_dir = "dbfs:/FileStore/tables"  # <-- Change if your files live elsewhere
zomato_path = f"{base_dir}/zomato.csv"
country_excel_path = f"{base_dir}/Country-Code.xlsx"  # Sheet1 expected
json_glob = f"{base_dir}/file*.json"  # matches file1.json ... file5.json

# Utility to browse the folder and verify files exist
print("Listing:", base_dir)
display(dbutils.fs.ls(base_dir))

# Helper: convert dbfs:/path → /dbfs/path (needed by pandas)
def dbfs_to_local(dbfs_path: str) -> str:
    return dbfs_path.replace("dbfs:/", "/dbfs/")

local_country_excel = dbfs_to_local(country_excel_path)

---

%md
## 3) Load CSV (Zomato)
- Infers schema and header
- Creates a temporary view for ad‑hoc SQL

---

%python
zomato_df = (
    spark.read.format("csv")
    .option("header", "true")
    .option("inferSchema", "true")
    .load(zomato_path)
)

print("Rows:", zomato_df.count())
zomato_df.printSchema()
display(zomato_df.limit(10))

# Temp view for quick SQL
zomato_df.createOrReplaceTempView("zomato_csv")

---

%md
## 4) Load Excel (Country Codes)
**Default approach:** pandas reads the Excel → convert to Spark DataFrame.

> If you have the Spark Excel connector installed (`com.crealytics.spark.excel`), you can use it directly (optional cell included below).

---

%python
# Install dependency for reading Excel via pandas if needed
# (No-op if already available)
%pip install openpyxl --quiet

import pandas as pd

country_pdf = pd.read_excel(local_country_excel, sheet_name="Sheet1")
print(country_pdf.head())

country_df = spark.createDataFrame(country_pdf)
country_df.printSchema()
display(country_df.limit(10))

# Temp view
country_df.createOrReplaceTempView("country_excel")

---

%md
### (Optional) Alternative: Spark Excel Connector
Use this only if your cluster has `com.crealytics:spark-excel` installed.

```python
country_df = (
    spark.read.format("com.crealytics.spark.excel")
    .option("header", "true")
    .option("inferSchema", "true")
    .option("dataAddress", "'Sheet1'!A1")
    .load(country_excel_path)
)
````

---

%md

## 5) Load JSON files (orders)

* Reads matching `file*.json` files
* If none are present, this step will skip gracefully

---

%python
from pyspark.sql.utils import AnalysisException

orders\_df = None
try:
\_df = spark.read.format("json").load(json\_glob)
\# If schema is empty or no files matched, .head(1) may raise; guard with action
if \_df.rdd.isEmpty():
print("No JSON files found at:", json\_glob)
else:
orders\_df = \_df
print("Orders rows:", orders\_df.count())
orders\_df.printSchema()
display(orders\_df.limit(10))
orders\_df.createOrReplaceTempView("orders\_json")
except AnalysisException as e:
print("Skipping JSON load:", e)

---

%md

## 6) Create a database and persist as Delta (Bronze)

* Database: `restaurant_demo`
* Tables: `zomato_bronze`, `country_codes_dim`, `orders_bronze` (if JSON present)

---

%sql
CREATE DATABASE IF NOT EXISTS restaurant\_demo;

---

%python
db = "restaurant\_demo"

(zomato\_df
.write.format("delta").mode("overwrite")
.saveAsTable(f"{db}.zomato\_bronze"))
print("Saved:", f"{db}.zomato\_bronze")

(country\_df
.write.format("delta").mode("overwrite")
.saveAsTable(f"{db}.country\_codes\_dim"))
print("Saved:", f"{db}.country\_codes\_dim")

if orders\_df is not None:
(orders\_df
.write.format("delta").mode("overwrite")
.saveAsTable(f"{db}.orders\_bronze"))
print("Saved:", f"{db}.orders\_bronze")
else:
print("No orders data to persist.")

---

%md

## 7) Build curated views (Silver)

* Normalize key Zomato columns
* Enrich with country names from the Excel dimension

> Column names are based on the common Kaggle Zomato schema (adjust if yours differs).

---

%sql
USE restaurant\_demo;

CREATE OR REPLACE VIEW zomato\_silver AS
SELECT
CAST(`Restaurant ID` AS STRING)                AS restaurant\_id,
`Restaurant Name`                              AS name,
`City`                                         AS city,
`Cuisines`                                     AS cuisines,
`Average Cost for two`                         AS avg\_cost\_for\_two,
`Price range`                                  AS price\_range,
`Aggregate rating`                             AS rating,
`Votes`                                        AS votes,
`Country Code`                                 AS country\_code
FROM zomato\_bronze;

CREATE OR REPLACE VIEW zomato\_enriched AS
SELECT z.\*, c.`Country` AS country
FROM zomato\_silver z
LEFT JOIN country\_codes\_dim c
ON z.country\_code = c.`Country Code`;

---

%md

## 8) Quality checks & quick exploration

---

%sql
\-- Row counts
SELECT 'zomato\_bronze' AS table\_name, COUNT(*) AS rows FROM zomato\_bronze
UNION ALL SELECT 'country\_codes\_dim', COUNT(*) FROM country\_codes\_dim
UNION ALL SELECT 'zomato\_enriched', COUNT(\*) FROM zomato\_enriched;

---

%sql
\-- Inspect schema
DESCRIBE TABLE EXTENDED zomato\_enriched;

---

%sql
\-- Top cuisines by average rating (min 50 votes)
SELECT
TRIM(cuisine) AS cuisine,
AVG(rating)   AS avg\_rating,
SUM(votes)    AS total\_votes,
COUNT(\*)      AS restaurant\_count
FROM (
SELECT EXPLODE(SPLIT(cuisines, ',')) AS cuisine, rating, votes
FROM zomato\_enriched
) x
GROUP BY TRIM(cuisine)
HAVING SUM(votes) >= 50
ORDER BY avg\_rating DESC, total\_votes DESC
LIMIT 20;

---

%sql
\-- Average cost for two by cuisine (showing 15)
SELECT
TRIM(cuisine) AS cuisine,
ROUND(AVG(avg\_cost\_for\_two), 2) AS avg\_cost\_for\_two
FROM (
SELECT EXPLODE(SPLIT(cuisines, ',')) AS cuisine, avg\_cost\_for\_two
FROM zomato\_enriched
) x
GROUP BY TRIM(cuisine)
ORDER BY avg\_cost\_for\_two DESC
LIMIT 15;

---

%sql
\-- Top cities by highly-rated restaurants (rating ≥ 4.5)
SELECT city, COUNT(\*) AS top\_rated\_count
FROM zomato\_enriched
WHERE rating >= 4.5
GROUP BY city
ORDER BY top\_rated\_count DESC
LIMIT 20;

---

%md

## 9) (Optional) Orders JSON — example analyses

> Run these only if `orders_bronze` exists. Adjust column names to your schema (e.g., `order_id`, `order_date`, `restaurant_id`, `total_amount`).

---

%sql
\-- Sanity checks
SHOW TABLES IN restaurant\_demo LIKE 'orders\_bronze';

---

%sql
\-- If your orders have `order_date` and `total_amount`
SELECT DATE(order\_date) AS order\_day,
COUNT(\*)         AS orders,
ROUND(SUM(total\_amount), 2) AS revenue
FROM orders\_bronze
GROUP BY DATE(order\_date)
ORDER BY order\_day;

---

%sql
\-- Join orders → restaurants (if `restaurant_id` keys match)
SELECT z.name, z.city, COUNT(o.\*) AS orders, ROUND(SUM(o.total\_amount),2) AS revenue
FROM orders\_bronze o
JOIN zomato\_silver z
ON CAST(o.restaurant\_id AS STRING) = z.restaurant\_id
GROUP BY z.name, z.city
ORDER BY revenue DESC
LIMIT 20;

---

%md

## 10) Housekeeping & Next Steps

* Promote curated datasets to **Gold** (BI-ready) as new Delta tables if needed
* Add constraints, Z-Order, and Optimize for faster queries on large data
* Schedule refresh as a **Job**

---

%sql
\-- Example: persist enriched view as a Delta table (Gold)
CREATE OR REPLACE TABLE restaurant\_demo.zomato\_gold AS
SELECT \* FROM restaurant\_demo.zomato\_enriched;

\-- Optional performance hygiene on larger datasets
\-- OPTIMIZE restaurant\_demo.zomato\_gold ZORDER BY (city, price\_range);

---

%md

### Common Gotchas

* **Paths**: If your files aren’t in `dbfs:/FileStore/tables`, update `base_dir` accordingly.
* **Excel**: If pandas cannot read the Excel, ensure `openpyxl` is installed (see cell with `%pip install openpyxl`).
* **Schema names**: Zomato column names may include spaces; use backticks `` `like this` `` in SQL.
* **JSON**: If schemas differ across files, consider `mergeSchema` or normalize to a common structure before writing.

---

%md

## You’re set!

* Explore with SQL against `restaurant_demo.zomato_enriched`
* Wire this into dashboards or downstream ML

```
```
