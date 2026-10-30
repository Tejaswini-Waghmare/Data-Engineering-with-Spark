# Data Engineering with Spark — 8-Module Learning Repository

A practical, **educational** learning path for PySpark, Spark SQL, ETL, data quality, Parquet, optimization, and a retail lakehouse-style pipeline. All datasets are **synthetic** and deliberately include invalid rows for validation exercises.

## Getting started

1. Use Python 3.10–3.12 and install Java 17 if required by your local Spark distribution.
2. Create a virtual environment: `python -m venv .venv` and activate it.
3. Install dependencies: `pip install -r requirements.txt`.
4. Start Jupyter: `jupyter notebook` and open a module's `Examples.ipynb`.
5. Run cells top to bottom. Notebooks use paths relative to the repository root; start Jupyter from the root folder.

For Google Colab, upload both CSV files and update the dataset paths in the notebooks to their uploaded locations. Installing PySpark in Colab may require a runtime restart.

## Modules

- [Data Engineering Fundamentals](./01_Data_Engineering_Fundamentals/README.md) — Data pipelines, batch vs streaming, schemas, orchestration, lineage, and idempotency.
- [PySpark Basics](./02_PySpark_Basics/README.md) — SparkSession, DataFrames, lazy evaluation, transformations and actions.
- [Spark SQL and Joins](./03_Spark_SQL_and_Joins/README.md) — SQL views, groupBy, join strategies, skew and partitioning.
- [ETL Pipelines](./04_ETL_Pipelines/README.md) — Extract, transform, load, reusable functions and incremental patterns.
- [Data Quality](./05_Data_Quality/README.md) — Validation, duplicates, null checks, quarantine and audit metrics.
- [Data Lakes and Parquet](./06_Data_Lakes_and_Parquet/README.md) — Parquet, partitioning, bronze/silver/gold and file formats.
- [Performance and Optimization](./07_Performance_and_Optimization/README.md) — Caching, explain plans, shuffle, broadcast and performance tradeoffs.
- [End-to-End Retail Pipeline](./08_End_to_End_Retail_Pipeline/README.md) — A complete bronze-to-silver-to-gold retail transaction pipeline.

## Dataset and quality policy

- `data/retail_orders_messy.csv`: 243 rows including one exact duplicate and several invalid values.
- `data/customers.csv`: 70 synthetic customer records.
- A *completed* order counts toward revenue only when date, quantity, price, and region pass validation.
- Invalid records are **quarantined**, not silently discarded; cancelled and returned orders are excluded from completed-order revenue.
- Results are demonstrations, not claims about any actual retailer.

## Portfolio integrity

These materials are generated learning examples. Run each notebook, inspect the outputs, document your own insights, and disclose synthetic data when showcasing work. Local Spark availability and Java compatibility may vary.
