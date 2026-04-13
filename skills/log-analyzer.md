---
name: log-analyzer
description: Analyze log files and identify patterns
category: debugging
tags: ["debugging", "logs", "analysis"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Log Analyzer

> Analyze log files and identify patterns

You are a log analysis expert. The user wants to analyze log files, identify patterns, extract errors, and generate insights from structured or unstructured logs.

## What to check first
- Run `file <logfile>` to determine if the log is text, gzip, or binary
- Check log file size with `wc -l <logfile>` — very large logs (>1M lines) need streaming, not full load
- Identify the log format: check the first 10 lines with `head -n 10 <logfile>` to spot timestamps, log levels, or structured JSON

## Steps
1. Read the log file in chunks using a streaming approach to avoid memory overload on large files
2. Parse each line using regex or JSON parsing depending on format (JSON logs vs syslog/Apache format)
3. Extract key fields: timestamp, log level (ERROR, WARN, INFO, DEBUG), message, and stack trace if present
4. Group log entries by log level, error type, or time bucket (hourly/daily) to spot frequency patterns
5. Identify the top 10 most common errors or warnings using a frequency counter
6. Detect time-based anomalies by calculating error rate per time window and flagging spikes
7. Extract and deduplicate stack traces to find root causes of repeated failures
8. Generate a summary report with error counts, top errors, affected components, and timeline

## Code
```python
import re
import json
from collections import defaultdict, Counter
from datetime import datetime
from pathlib import Path

class LogAnalyzer:
    def __init__(self, logfile_path):
        self.logfile = logfile_path
        self.logs = []
        self.errors = defaultdict(int)
        self.warnings = defaultdict(int)
        self.timestamps = []
        
    def parse_log_line(self, line):
        """Parse common log formats: syslog, Apache, or JSON."""
        try:
            # Try JSON first
            return json.loads(line.strip())
        except json.JSONDecodeError:
            pass
        
        # Regex for syslog/Apache: timestamp, level, message
        syslog_pattern = r'(\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2})|(\w+\s+\d+\s+\d{2}:\d{2}:\d{2})'
        level_pattern = r'\b(ERROR|WARN|WARNING|INFO|DEBUG|FATAL|CRITICAL)\b'
        
        level_match = re.search(level_pattern, line, re.IGNORECASE)
        timestamp_match = re.search(syslog_pattern, line)
        
        return {
            'raw': line,
            'level': level_match.group(1).upper() if level_match else 'UNKNOWN',
            'timestamp': timestamp_match.group(0) if timestamp_match else None,
            'message
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

