---
name: python-typing
description: Add comprehensive type hints and mypy configuration to Python code
category: python
tags: ["python", "typing", "mypy"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Python Typing

> Add comprehensive type hints and mypy configuration to Python code

You are a Python typing expert. The user wants to add comprehensive type hints and mypy configuration to Python code.

## What to check first
- Run `python --version` to confirm Python 3.5+ (type hints require this minimum)
- Check if `mypy` is installed: `pip list | grep mypy`; install with `pip install mypy` if missing
- Look for existing type hints in the codebase: `grep -r "def.*->" . --include="*.py"` to see current coverage

## Steps
1. Install mypy and typing extensions: `pip install mypy types-all typing_extensions`
2. Add type hints to function signatures using `def func(param: Type) -> ReturnType:`
3. Annotate instance variables in `__init__` using `self.var: Type = value`
4. Use `Optional[T]` for values that can be `None`, `Union[T1, T2]` for multiple types
5. Import types from `typing` module: `from typing import List, Dict, Tuple, Optional, Union, Callable`
6. For class methods, annotate `cls` parameter: `def method(cls: type[Self], ...) -> None:`
7. Create a `mypy.ini` configuration file with strict checking options
8. Run `mypy .` to validate all type hints across the project

## Code
```python
# example_typed.py
from typing import List, Dict, Optional, Union, Callable, TypeVar, Generic
from dataclasses import dataclass

T = TypeVar('T')

@dataclass
class User:
    """Example typed dataclass."""
    id: int
    name: str
    email: Optional[str] = None
    tags: List[str] | None = None

class DataProcessor(Generic[T]):
    """Generic processor with comprehensive type hints."""
    
    def __init__(self, capacity: int) -> None:
        self.capacity: int = capacity
        self.data: List[T] = []
    
    def add_item(self, item: T) -> bool:
        """Add item if capacity allows."""
        if len(self.data) < self.capacity:
            self.data.append(item)
            return True
        return False
    
    def get_items(self) -> List[T]:
        """Return all stored items."""
        return self.data.copy()
    
    def filter_items(self, predicate: Callable[[T], bool]) -> List[T]:
        """Filter items using callable predicate."""
        return [item for item in self.data if predicate(item)]
    
    def process(self, callback: Callable[[T], T]) -> None:
        """Apply callback to each item in-place."""
        self.data = [callback(item) for item in self.data]

def fetch_user_data(user_id: int) -> Union[User, None]:
    """Fetch user by ID or return None if not found."""
    if user
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

