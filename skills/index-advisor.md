---
name: index-advisor
description: Suggest database indexes based on query patterns
category: database
tags: ["database", "indexes", "performance"]
difficulty: advanced
version: 1.0.0
author: Claude Skills Hub
---

# Index Advisor

> Suggest database indexes based on query patterns

You are a database performance specialist. The user wants to analyze query patterns and recommend indexes that will improve database performance.

## What to check first
- Run `SHOW PROCESSLIST;` or check slow query logs (`/var/log/mysql/slow.log` or PostgreSQL's `log_statement = 'all'`) to capture actual queries
- Verify `long_query_time` is set appropriately (e.g., `SET GLOBAL long_query_time = 0.5;`) to capture slow queries
- Check table schema with `DESCRIBE table_name;` or `\d table_name` (PostgreSQL) to understand column types and existing indexes

## Steps
1. Extract slow queries from logs or `SHOW PROCESSLIST` and identify WHERE, JOIN, and ORDER BY clauses
2. Use `EXPLAIN` (MySQL) or `EXPLAIN ANALYZE` (PostgreSQL) on each query to see full execution plan and identify sequential scans or high row counts
3. Count query frequency—prioritize indexing for queries that run most often or have longest execution times
4. Identify columns used in WHERE conditions, JOIN conditions, and ORDER BY in high-impact queries
5. Check existing indexes with `SHOW INDEX FROM table_name;` or `\d table_name` to avoid duplicates
6. For multi-column predicates, determine index column order: equality conditions first, then range/inequality conditions, then ORDER BY columns
7. Create candidate indexes with `CREATE INDEX idx_name ON table_name (col1, col2);` and re-run EXPLAIN to verify improvement
8. Validate index impact using query execution time before/after and check index size (`SELECT * FROM information_schema.STATISTICS;`)

## Code
```python
import re
import subprocess
from typing import List, Dict, Tuple

class IndexAdvisor:
    def __init__(self, db_type: str = "mysql"):
        self.db_type = db_type
        self.queries = []
        self.index_recommendations = []
    
    def parse_slow_log(self, log_file: str) -> List[str]:
        """Extract SQL queries from slow query log."""
        queries = []
        with open(log_file, 'r') as f:
            content = f.read()
        
        # MySQL slow log format: # Time: ... # User@Host: ...
        query_blocks = re.split(r'# Time:', content)
        
        for block in query_blocks[1:]:
            lines = block.strip().split('\n')
            sql_lines = []
            for line in lines:
                if line and not line.startswith('#'):
                    sql_lines.append(line)
            if sql_lines:
                query = ' '.join(sql_lines).strip()
                queries.append(query)
        
        return queries
    
    def extract_columns_from_query(self, query: str) -> Dict[str, List[str]]:
        """Extract WHERE, JOIN, and ORDER BY columns."""
        columns
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

