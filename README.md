# PySpark Real-Time

<p>This project demonstrates how to build real-time data pipelines with PySpark on Databricks. It covers key patterns such as data ingestion, JSON flattening, deduplication, incremental upserts, streaming writes, and Slowly Changing Dimension (SCD) Type 2 implementation.</p>

<p>The notebooks are modular, focusing on reusable transformation classes and practical use cases for production-ready pipelines.</p>


## 📂 Project Structure
- **Reuse Transformation Class**
  <p>Encapsulates common PySpark transformations (e.g., deduplication, validations) into reusable classes for clean and maintainable pipelines.</p>

- **Loading**
  <p>Demonstrates incremental load with UPSERT logic, handling changing data efficiently by merging new records with existing tables.</p>

- **Json Processing**
  <p>Provides utilities to flatten nested JSON structures into tabular formats for easier downstream analysis.</p>

- **Streaming Table**
  <p>Implements Delta Lake streaming writes, with checkpointing and schema evolution support.</p>

- **SCD Type 2**
  <p>Implements Slowly Changing Dimension Type 2 for tracking historical changes in dimension tables.</p>

- **Lakeform**
  <p>Directory placeholder (likely for Delta Lake-formatted outputs).</p>


## 🚀 Key Features & Code Snippets
1. **Reusable Transformation Class**
Reusable class for deduplication and data cleaning:
```py
from pyspark.sql.functions import col, row_number
from pyspark.sql.window import Window

class DataValidation:

    def __init(self, df):
        self.dataframe = df

    def deduplication(self, partitionBy, orderBy):
        return self.dataframe.withColumn(
            "dedup", row_number().over(Window.partitionBy(partitionBy).orderBy(orderBy))
        ).filter(col("dedup") == 1).drop("dedup")

    def remove_nulls(self, nullColumn):
        return self.dataframe.filter(col(nullColumn).isNotNull())

```

---
2. **Streaming Writes to Delta**
Structured Streaming with Delta Lake output:
```py
df.writeStream \
  .format("delta") \
  .option("checkpointLocation", "/Volumes/pysparkrealtime/source/real-time/stream/checkpoint") \
  .option("mergeSchema", "true") \
  .trigger(once=True) \
  .start("/Volumes/pysparkrealtime/source/real-time/stream/data")
```

---

3. **Slowly Changing Dimension (SCD Type 2)**

Creating a base table for SCD Type 2:
```sql
CREATE TABLE pysparkrealtime.source.customers
(
  id INT,
  email STRING,
  city STRING,
  country STRING,
  modifiedDate TIMESTAMP
)
```
<p>This enables tracking of historical changes in customer dimension data.</p>

---

## ⚙️ Technologies Used

* Apache Spark (PySpark)

* Databricks

* Delta Lake

* SQL + Python integration

---

## ▶️ How to Use
1. Import the `.dbc` archive into your Databricks Workspace.
  * Navigate to **Workspace** → **Import** → **Upload DBC**.
  * Select `PySpark Real-Time.dbc`.

2. Run notebooks in the following order for end-to-end pipeline:
  * `Reuse Transformation Class`
  * `Loading`
  * `Json Processing`
  * `Streaming Table`
  * `SCD Type 2`

Review the output Delta tables under your configured workspace paths.

---
## 📝 Notes

* This project is designed as a teaching/reference pipeline and can be adapted for production workloads.
* Ensure your Delta paths and checkpoint directories are updated before execution.
* Lakeform is a placeholder folder for Lakehouse-formatted data.



