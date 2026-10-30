# PySpark Basics — Detailed Study Notes

## Learning goals

SparkSession, DataFrames, lazy evaluation, transformations and actions.

## Core concepts

**Spark execution model:** Spark distributes work across partitions. DataFrame transformations are lazy: execution starts when an action such as `count`, `show`, or `write` is called. The optimizer constructs a physical plan; inspect it using `explain()`.

**Schemas:** Explicit types prevent accidental string arithmetic and ambiguous dates. CSV schema inference is convenient for exploration, but production jobs should use a declared `StructType` and validated ingestion rules.

**Correctness:** Distinguish exact duplicates from business-key conflicts. Missing or invalid dates, nonpositive quantities, negative prices, and missing regions must be handled with documented policies. Use a quarantine output to preserve rejected records for investigation.

**Revenue definition:** `quantity * unit_price` for valid completed orders only. Returns and cancellations are excluded. Revenue is aggregated after validation, not before.

**Reproducibility:** Use fixed input files, deterministic transformations, clear data contracts, and explicit write modes. Re-running the notebook should produce consistent output counts. `mode("overwrite")` is appropriate for local demos but needs stronger controls in production.

**Performance:** Filter early when possible, avoid collecting large data to the driver, inspect shuffle-heavy joins, and use broadcast joins only when a dimension table is genuinely small. Cache only reused DataFrames and unpersist afterward.

**Limitations:** Synthetic retail data cannot establish real-world operational performance, and small local runs cannot predict cluster performance.

## Key code pattern

```python
from pyspark.sql import functions as F
orders = spark.read.option("header", True).option("inferSchema", True).csv(orders_path)
orders.groupBy("region").agg(F.count("*").alias("n")).show()
```

## Checks to perform

1. Confirm the raw record count and column types.
2. Identify nulls, invalid values, and duplicates before aggregation.
3. Assert `accepted + quarantined == deduplicated`.
4. Verify that returned and cancelled orders do not contribute to completed revenue.
5. Explain how partitioning, shuffles, and caching could affect a larger dataset.

## Further reading

- Apache Spark PySpark DataFrame API: https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/index.html
- Spark SQL guide: https://spark.apache.org/docs/latest/sql-programming-guide.html
- Spark tuning guide: https://spark.apache.org/docs/latest/sql-performance-tuning.html
