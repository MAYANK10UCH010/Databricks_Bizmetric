# 📊 Pandas vs Spark DataFrame – Basic Functions

| Task | Pandas (DataFrame `df`) | Spark (DataFrame `sdf`) |
|------|--------------------------|--------------------------|
| Show first 5 rows | `df.head()` | `sdf.show(5)` |
| Show first n rows | `df.head(n)` | `sdf.show(n)` |
| Show last 5 rows | `df.tail()` | ⚠️ Not directly available. Workaround: `sdf.orderBy(F.desc("col")).show(5)` |
| Display random sample | `df.sample(n=5)` | `sdf.sample(fraction=0.1).show()` |
| Column names | `df.columns` | `sdf.columns` |
| Number of columns | `len(df.columns)` | `len(sdf.columns)` |
| Data types | `df.dtypes` | `sdf.dtypes` |
| Schema (nice formatted dtypes) | `df.info()` | `sdf.printSchema()` |
| Shape (#rows, #cols) | `df.shape` | `(sdf.count(), len(sdf.columns))` |
| Number of rows | `len(df)` or `df.shape[0]` | `sdf.count()` |
| Select a column | `df["col"]` | `sdf.select("col")` |
| Select multiple columns | `df[["col1","col2"]]` | `sdf.select("col1","col2")` |
| Unique values in a column | `df["col"].unique()` | `sdf.select("col").distinct().show()` |
| Value counts | `df["col"].value_counts()` | `sdf.groupBy("col").count().show()` |
| Describe stats | `df.describe()` | `sdf.describe().show()` |
| Drop duplicates | `df.drop_duplicates()` | `sdf.dropDuplicates()` |
| Filter rows | `df[df["col"] > 10]` | `sdf.filter(sdf["col"] > 10)` |
| Sort values | `df.sort_values("col")` | `sdf.orderBy("col")` |
