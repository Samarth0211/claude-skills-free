---
name: test-coverage-analyzer
description: Analyze test coverage gaps and suggest tests to write
category: testing
tags: ["testing", "coverage", "analysis"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Test Coverage Analyzer

> Analyze test coverage gaps and suggest tests to write

---
You are a test coverage analyst. The user wants to analyze test coverage gaps in their codebase and get specific suggestions for which tests to write next.

## What to check first
- Run `coverage run -m pytest` or `pytest --cov=<module>` to generate coverage data
- Check for the `.coverage` file or `coverage.xml` output file
- Inspect `htmlcov/index.html` if HTML reports were generated
- Look at `pyproject.toml` or `setup.cfg` for coverage thresholds and exclusions

## Steps
1. Execute `coverage run -m pytest --cov=<your_module> --cov-report=html --cov-report=term-missing` to generate detailed coverage metrics and identify uncovered lines
2. Parse the terminal output or open `htmlcov/index.html` in a browser to visualize which lines/branches are missing coverage
3. Run `coverage json --pretty-print` to generate machine-readable coverage data for analysis
4. Identify patterns in uncovered code: error handling paths, edge cases in conditionals, exception handlers, and rarely-used branches
5. Use `coverage report --skip-empty --precision=2` to get a summary of modules ranked by coverage percentage
6. Create a prioritized list of untested code segments, focusing first on high-complexity functions and critical paths
7. Write new test cases targeting the identified gaps, using the code template provided
8. Re-run coverage analysis to verify improvements and track progress toward your coverage target

## Code
```python
import json
import subprocess
import sys
from pathlib import Path
from typing import Dict, List, Tuple

class CoverageAnalyzer:
    def __init__(self, module_name: str, threshold: int = 80):
        self.module_name = module_name
        self.threshold = threshold
        self.coverage_data = {}
    
    def run_coverage(self) -> bool:
        """Execute pytest with coverage and generate JSON report."""
        cmd = [
            "coverage", "run", "-m", "pytest",
            f"--cov={self.module_name}",
            "--cov-report=json",
            "--cov-report=html"
        ]
        result = subprocess.run(cmd, capture_output=True, text=True)
        if result.returncode != 0:
            print(f"Coverage run failed: {result.stderr}")
            return False
        return True
    
    def load_coverage_json(self) -> Dict:
        """Load and parse coverage.json file."""
        try:
            with open(".coverage", "r") as f:
                pass
        except FileNotFoundError:
            pass
        
        try:
            with open("coverage.json", "r") as f:
                self.coverage_data = json.load(f)
                return self.coverage_data
        except FileNotFoundError:
            print("coverage.json not found. Run coverage first.")
            return {}
    
    def analyze_gaps(
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

