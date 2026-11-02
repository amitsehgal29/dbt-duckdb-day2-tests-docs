# dbt-duckdb-day2-tests-docs

Day 2 of a 7-day dbt + Airflow series: tests and docs on top of the [day 1 skeleton](https://github.com/amitsehgal29/dbt-duckdb-day1-skeleton). It runs locally on DuckDB, with no warehouse or cloud account.

## What's new compared with day 1

- A second seed, `raw_customers.csv`, and a `stg_customers` model.
- `models/staging/schema.yml` with model and column descriptions and generic tests: `not_null`, `unique`, `accepted_values`, and `relationships` (every order's `customer_id` must exist in `stg_customers`).
- A doc block in `models/staging/docs.md` that explains each `order_status` value, used in the column description.
- A singular test, `tests/assert_amount_sign_matches_status.sql`: returned orders must be negative, cancelled orders zero, and all other orders positive.

## Run it

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt          # dbt-core + dbt-duckdb
cp profiles.yml.example profiles.yml
dbt deps --profiles-dir .                # no packages yet, safe to run
dbt seed --profiles-dir .
dbt build --profiles-dir .               # runs models and all tests
dbt docs generate --profiles-dir .
dbt docs serve --profiles-dir .          # opens the docs site on localhost:8080
```

To see a test fail, change a `-89.00` in `seeds/raw_orders.csv` to `89.00` and run `dbt build` again.

## The series

1. dbt skeleton on DuckDB
2. Tests and docs (this repo)
3. Incremental model and SCD2 snapshot
4. Airflow DAG running dbt
5. Sensors, retries and alerts
6. Data-quality gates
7. End-to-end demo
