# Unity Catalog with Delta Lake

## 🔹 Functionality

### **Unity Catalog**
- Central **governance and access control layer** in Databricks.  
- Provides **fine-grained permissions** (catalog → schema → table → column → row).  
- Unified metadata store for **all data + AI assets** (tables, views, ML models, GenAI assets).  
- Ensures **security, audit, lineage** across multi-cloud & multi-user setups.

### **Delta Lake**
- **Storage format + transaction log layer** on top of cloud object storage.  
- Ensures **ACID transactions**, **schema enforcement**, **time travel**, and **data reliability**.  
- Basis for building **data lakes / lakehouses**.

---

## 🔹 How They Work Together
- **Delta Lake** ensures **reliable and consistent data storage**.  
- **Unity Catalog** governs **who can access, query, or modify that Delta table** and **tracks lineage**.  

👉 **Delta Lake** = *data format + reliability*  
👉 **Unity Catalog** = *security + governance + lineage*  

---

## 🔹 Example: Enterprise Scenario

### Use Case: Customer Data Platform with GenAI
1. **Data Ingestion**:  
   Raw customer interaction logs land in cloud storage → ingested into **Delta Lake tables** (`bronze.customers`).

2. **Data Processing**:  
   ETL transforms create **clean Delta tables** (`silver.customers_clean`, `gold.customer_360`).

3. **Governance via Unity Catalog**:  
   - Data engineer defines catalog: `company_data`.  
   - Schemas inside: `bronze`, `silver`, `gold`.  
   - Delta tables registered under schemas.  
   - Unity Catalog permissions:  
     - Marketing analysts → `SELECT` on `gold.customer_360`.  
     - Data scientists → `SELECT` on `silver.customers_clean`.  
     - No direct access to raw `bronze`.  
   - Column masking (e.g., hash customer emails, mask phone numbers).  
   - Lineage tracking shows `gold.customer_360` was derived from `bronze.customers`.  

4. **GenAI Application**:  
   - A chatbot powered by LLM uses `gold.customer_360` (governed via Unity Catalog).  
   - Every query logged for audit.  
   - Unity Catalog ensures **only anonymized columns** are exposed to GenAI APIs.  

---

## 🔹 Code Example (PySpark in Databricks)

```python
# Create Catalog & Schema
spark.sql("CREATE CATALOG company_data")
spark.sql("CREATE SCHEMA company_data.gold")

# Create Delta Table in Unity Catalog
spark.sql("""
CREATE TABLE company_data.gold.customer_360
USING DELTA
LOCATION 's3://bucket/company/gold/customer_360'
AS SELECT * FROM silver.customers_clean
""")

# Apply fine-grained access control
spark.sql("GRANT SELECT ON TABLE company_data.gold.customer_360 TO `marketing_analyst`")
spark.sql("GRANT SELECT ON TABLE company_data.silver.customers_clean TO `data_scientist`")
```
### 🔹In Summary
      -🔹Delta Lake: Gives trustworthy, ACID-compliant data storage, time-travel,Z ordering. 
      -🔹Unity Catalog: Provides governance, access control, lineage, and security over that Delta data.
      -🔹Together → They enable a secure, compliant, enterprise-scale Lakehouse for GenAI + BI + ML workloads.

### 🔹Delta Lake Core Capabilities

- ACID transactions → guarantees consistency in concurrent reads/writes.
- Schema enforcement & evolution → prevents corrupt/incompatible data.
- Time travel → query data at previous versions using transaction logs.

### 🔹Data reliability → built-in handling for late arriving or batch/stream data. Performance Features (optional but common)

- Z-Ordering → a technique Delta Lake supports to improve query performance on large datasets (by colocating related data).
- Data skipping → improves read performance by skipping irrelevant files.
- Compaction (OPTIMIZE) → combines small files for efficiency.
