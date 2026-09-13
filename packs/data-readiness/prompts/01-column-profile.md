# 🔬 01 · Column profile

> Writes the profiling query for your SQL dialect

[![Pack](https://img.shields.io/badge/pack-data%20readiness-5DBBD6?labelColor=0B2039)](../README.md)
[![Step](https://img.shields.io/badge/step-01%20of%2006-2F5F7B?labelColor=0B2039)](../README.md#-use-them-in-order)
[![Claude](https://img.shields.io/badge/Claude-tested-5DBBD6?labelColor=0B2039)](#-tested-on)
[![Others](https://img.shields.io/badge/other%20models-not%20yet%20tested-8FA3B8?labelColor=0B2039)](#-tested-on)
[![Episode](https://img.shields.io/badge/episode-EP--001-F2A93B?labelColor=0B2039)](#)

`#DataQuality` `#AIReadiness` `#SQL` `#PromptEngineering` `#NeumannTechTips`

[🏠 Home](../../../README.md) · [📦 Pack index](../README.md) · ⬅️ _first in the pack_ · [02 Null and default scan ➡️](02-null-and-default-scan.md)

---

| | |
| :-- | :-- |
| 🎯 **Use it for** | Producing the profiling query that tells you what is actually in a table, before anyone argues about whether it is ready for AI. |
| 📥 **You need** | The table name, the column list with declared types, and your SQL dialect. |
| 📤 **Returns** | A single query returning one row per column: row count, null count and rate, distinct count, cardinality ratio, minimum, maximum, and the five most frequent values. |

---

## 💬 The prompt

```
You are helping me profile a table before it is considered for AI model
training. I will not paste any data to you. Write the query; I will run it.

Dialect: [Postgres | SQL Server | Oracle | BigQuery | Snowflake | DuckDB]
Table: [schema.table]
Columns and declared types:
[paste the DDL or a column/type list]

Write ONE query that returns one row per column with these fields:
  column_name, declared_type, row_count, null_count, null_rate,
  distinct_count, cardinality_ratio (distinct/non-null),
  min_value, max_value, top_5_values_with_counts

Requirements:
- Aggregate only. Do not return any individual record.
- Cast values to text where needed so all columns fit one result shape.
- It must run in a single pass over the table where the dialect allows it.
- Add a brief comment above each block explaining what it detects.
- If the dialect cannot do something in one query, say so and give the
  minimum number of queries instead, numbered in run order.

Do not interpret the results. Do not speculate about what you will find.
Return the query and nothing else.
```

---

## 🧪 Worked example

**Input given to the model**

```
Dialect: Postgres
Table: staging.customer
Columns and declared types:
  customer_id    bigint
  first_name     varchar(80)
  last_name      varchar(80)
  email          varchar(255)
  mobile         varchar(40)
  date_of_birth  date
  status_code    varchar(10)
  created_at     timestamp
```

**Shape of what came back** (abbreviated)

```sql
-- One row per column. Aggregate only: no individual records are returned.
WITH totals AS (SELECT count(*) AS row_count FROM staging.customer)
SELECT 'first_name' AS column_name,
       'varchar(80)' AS declared_type,
       t.row_count,
       count(*) FILTER (WHERE c.first_name IS NULL) AS null_count,
       round(count(*) FILTER (WHERE c.first_name IS NULL)::numeric
             / nullif(t.row_count,0), 4) AS null_rate,
       count(DISTINCT c.first_name) AS distinct_count,
       ...
FROM staging.customer c CROSS JOIN totals t
GROUP BY t.row_count
UNION ALL
-- repeated per column
```

The top-five block came back as a separate numbered query, with the model
stating that Postgres cannot return per-column mode values inside the same
aggregate without a lateral join per column.

---

## ⚠️ Known limits

| Limit | What to do |
| --- | --- |
| On very wide tables the single query becomes unwieldy and slow | Ask for the query in batches of ten columns |
| Models will happily invent dialect-specific functions that do not exist | Run it. A syntax error is cheap; a silently wrong result is not |
| `min` and `max` on a free text column are rarely meaningful | Ignore those two fields for free text and rely on the top five |
| Cardinality ratio on a billion-row table can be expensive | Ask for an approximate count function where your dialect has one |

> [!WARNING]
> Read the query before you run it against production. A model asked for a single pass will sometimes give you a cross join that is anything but.

---

## 🤖 Tested on

| Model | Result | Note |
| --- | --- | --- |
| Claude | ✅ | Correct Postgres, flagged the lateral join limitation unprompted |
| ChatGPT | ⏳ | Not yet tested |
| Gemini | ⏳ | Not yet tested |
| Grok | ⏳ | Not yet tested |
| Microsoft Copilot | ⏳ | Not yet tested |
| Open-weight, local | ⏳ | Not yet tested |

---

**Episode** · EP-001, *Six checks before you call your data AI-ready*

<div align="center">

⬅️ _first in the pack_ · [📦 Pack index](../README.md) · [02 Null and default scan ➡️](02-null-and-default-scan.md)

**[NeumannTechTips](https://www.youtube.com/@NeumannTechTips)** · Practical AI. Real results.

</div>
