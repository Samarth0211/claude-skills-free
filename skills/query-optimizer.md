---
name: query-optimizer
description: Analyze and optimize slow database queries
category: database
tags: ["database", "optimization", "performance"]
difficulty: advanced
version: 1.0.0
author: Claude Skills Hub
---

# Query Optimizer

> Analyze and optimize slow database queries

You are a database performance engineer specializing in query analysis and optimization. The user wants to analyze slow database queries and implement optimization strategies.

## What to check first
- Run `SHOW PROCESSLIST;` or `SELECT * FROM pg_stat_statements;` (PostgreSQL) to identify currently running queries and their execution times
- Execute `EXPLAIN ANALYZE` or `EXPLAIN (ANALYZE, BUFFERS)` on the suspect query to examine the query plan and actual row counts
- Check table statistics with `ANALYZE table_name;` to ensure the optimizer has current information

## Steps
1. Enable query logging by setting `slow_query_log=1` and `long_query_time=2` (MySQL) or `log_min_duration_statement=1000` (PostgreSQL in milliseconds)
2. Capture the slow query's execution plan using `EXPLAIN ANALYZE SELECT ...` and review node types (Seq Scan vs Index Scan, nested loop costs)
3. Identify missing indexes by examining Filter conditions and WHERE clauses in the execution plan that perform sequential scans
4. Create targeted indexes using `CREATE INDEX idx_name ON table(column1, column2)` matching the query's join conditions and WHERE predicates
5. Check for statistics staleness with `SELECT last_analyze FROM pg_stat_user_tables;` and run `ANALYZE;` if needed
6. Rewrite the query to avoid subqueries in SELECT clause—use JOINs or CTEs (WITH clauses) instead
7. Review JOIN order by checking the EXPLAIN plan; reorder tables in the query if the optimizer chose an inefficient order
8. Measure improvement by comparing execution time before and after with `\timing on` (psql) or `SET profiling=1;` (MySQL)

## Code
```sql
-- Complete query optimization workflow
-- Step 1: Capture baseline performance
EXPLAIN (ANALYZE, BUFFERS, VERBOSE) 
SELECT 
  o.order_id,
  o.order_date,
  c.customer_name,
  p.product_name,
  oi.quantity,
  oi.unit_price
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN order_items oi ON o.order_id = oi.order_id
INNER JOIN products p ON oi.product_id = p.product_id
WHERE o.order_date >= '2024-01-01'
  AND c.country = 'USA'
  AND p.category_id = 5
ORDER BY o.order_date DESC
LIMIT 100;

-- Step 2: Create composite indexes for join conditions
CREATE INDEX idx_orders_customer_date 
  ON orders(customer_id, order_date DESC) 
  INCLUDE (order_id);

CREATE INDEX idx_order_items_product 
  ON order_items(product_id, order_id);

CREATE INDEX idx_customers_country 
  ON customers(country, customer_id);

CREATE INDEX idx_
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Treating this skill as a one-shot solution — most workflows need iteration and verification
- Skipping the verification steps — you don't know it worked until you measure
- Applying this skill without understanding the underlying problem — read the related docs first


## When NOT to Use This Skill

- When a simpler manual approach would take less than 10 minutes
- On critical production systems without testing in staging first
- When you don't have permission or authorization to make these changes


## How to Verify It Worked

- Run the verification steps documented above
- Compare the output against your expected baseline
- Check logs for any warnings or errors — silent failures are the worst kind


## Production Considerations

- Test in staging before deploying to production
- Have a rollback plan — every change should be reversible
- Monitor the affected systems for at least 24 hours after the change



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

