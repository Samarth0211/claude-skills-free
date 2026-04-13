---
name: fastapi-setup
description: Scaffold FastAPI with async endpoints and auto-docs
category: python
tags: ["python", "fastapi", "async"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# FastAPI Setup

> Scaffold FastAPI with async endpoints and auto-docs

You are a Python backend developer. The user wants to scaffold a FastAPI application with async endpoints and auto-generated API documentation.

## What to check first
- Run `python --version` to ensure Python 3.7+ is installed
- Check if `pip` is available with `pip --version`
- Verify no existing FastAPI project in the current directory

## Steps
1. Create a new project directory and initialize a virtual environment with `python -m venv venv`
2. Activate the virtual environment: `source venv/bin/activate` (on Windows: `venv\Scripts\activate`)
3. Install FastAPI and Uvicorn with `pip install fastapi uvicorn[standard]`
4. Create `main.py` and import `FastAPI` from the fastapi module
5. Instantiate a FastAPI app with `app = FastAPI(title="Your API", version="1.0.0")`
6. Define async route handlers using `@app.get()`, `@app.post()`, etc. decorators with async def functions
7. Add Pydantic BaseModel classes for request/response validation with type hints
8. Run the development server with `uvicorn main:app --reload` to enable hot-reload and auto-docs at `/docs`

## Code
```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import Optional
import uvicorn

app = FastAPI(
    title="FastAPI App",
    description="A scalable async API with auto-generated documentation",
    version="1.0.0"
)

class Item(BaseModel):
    id: int
    name: str
    description: Optional[str] = None
    price: float

class ItemUpdate(BaseModel):
    name: Optional[str] = None
    description: Optional[str] = None
    price: Optional[float] = None

items_db = {}

@app.get("/", tags=["Root"])
async def read_root():
    """Root endpoint that returns a welcome message."""
    return {"message": "Welcome to FastAPI"}

@app.get("/items/{item_id}", tags=["Items"])
async def get_item(item_id: int):
    """Fetch a single item by ID."""
    if item_id not in items_db:
        raise HTTPException(status_code=404, detail="Item not found")
    return items_db[item_id]

@app.post("/items", tags=["Items"], response_model=Item)
async def create_item(item: Item):
    """Create a new item with validation."""
    items_db[item.id] = item
    return item

@app.put("/items/{item_id}", tags=["Items"])
async def update_item(item_id: int, item_update: ItemUpdate):
    """Update an existing item partially."""
    if item_id not in items_db:
        raise HTTPException(status_code=404, detail="Item not found")
    existing =
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

