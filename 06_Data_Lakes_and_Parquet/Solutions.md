# Worked Solutions — Data Lakes and Parquet

The examples below assume you ran the setup cell from `Examples.ipynb`.
## 1. Explain the difference between a transformation and an action in Spark.

Transformations build a logical plan (e.g. `filter`); actions trigger computation (e.g. `count`).

## 2. Load the orders CSV and report its row count and schema.

`orders.count()` and `orders.printSchema()`.

## 3. Find null or blank values in the region field.

`orders.filter(F.col("region").isNull() | (F.trim("region") == "")).count()`.

## 4. Count the number of exact duplicate records.

`orders.count() - orders.dropDuplicates().count()`.

## 5. Parse order_date and identify invalid dates.

`orders.withColumn("parsed_date", F.to_date("order_date", "yyyy-MM-dd")).filter(F.col("parsed_date").isNull()).show()`.

## 6. Identify nonpositive quantities and negative prices.

`orders.filter((F.col("quantity") <= 0) | (F.col("unit_price") < 0)).show()`.

## 7. Create an accepted DataFrame using explicit quality rules.

Use the `normalized`, `valid_expr`, `deduped`, and `accepted` definitions in the notebook quality-gates cell.

## 8. Create a quarantine DataFrame with rejected rows.

`quarantine = deduped.filter(~valid_expr)`.

## 9. Assert that accepted and quarantined rows reconcile to deduplicated input.

`assert accepted.count() + quarantine.count() == deduped.count()`.

## 10. Calculate completed-order revenue by region.

Filter `status == "completed"`, create `revenue = quantity_num * unit_price_num`, and group by `region_clean`; see notebook.

## 11. Join the orders and customer dimension and count rows by segment.

`orders.join(customers, "customer_id", "left").groupBy("segment").count().show()`.

## 12. Write accepted records to Parquet and read them back.

`accepted.write.mode("overwrite").parquet("outputs/accepted")`; verify with `spark.read.parquet("outputs/accepted").count()`.
