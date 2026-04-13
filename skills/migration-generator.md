---
name: migration-generator
description: Generate database migration files
category: database
tags: ["database", "migrations", "schema"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Migration Generator

> Generate database migration files

You are a database migration expert. The user wants to generate database migration files for schema changes with proper naming, timestamps, and rollback support.

## What to check first
- Run `npm list` or `pip list` to verify your migration tool is installed (Knex, Alembic, Sequelize, TypeORM, etc.)
- Check your current migrations directory exists at the path specified in your config (e.g., `./migrations`, `./db/migrations`)
- Confirm your database connection is configured and accessible via environment variables or config file

## Steps
1. Identify your migration tool and framework (check `package.json`, `requirements.txt`, or `tsconfig.json`)
2. Run the generate/create command with a descriptive snake_case name (e.g., `npx knex migrate:make add_users_table`)
3. Inspect the generated migration file to verify the timestamp prefix and empty up/down methods
4. Write the schema change in the `up` method using your tool's DSL (e.g., `table.increments('id')`, `ALTER TABLE`)
5. Write the reverse operation in the `down` method to safely rollback
6. Test the migration with `npx knex migrate:latest` to ensure it applies without errors
7. Verify rollback works with `npx knex migrate:rollback` and check the schema reverted
8. Commit the migration file with a clear message referencing the schema change

## Code
```javascript
// Knex.js migration generator example
// Run: npx knex migrate:make add_products_table
// File: migrations/20240115143022_add_products_table.js

exports.up = function(knex) {
  return knex.schema.createTable('products', function(table) {
    table.increments('id').primary();
    table.string('name', 255).notNullable();
    table.text('description');
    table.decimal('price', 10, 2).notNullable();
    table.integer('stock').defaultTo(0);
    table.enum('status', ['active', 'inactive', 'archived']).defaultTo('active');
    table.timestamp('created_at').defaultTo(knex.fn.now());
    table.timestamp('updated_at').defaultTo(knex.fn.now());
    table.index('name');
  });
};

exports.down = function(knex) {
  return knex.schema.dropTableIfExists('products');
};

// Alembic (SQLAlchemy/Python) migration example
// Run: alembic revision --autogenerate -m "add products table"
# versions/001_add_products_table.py

from alembic import op
import sqlalchemy as sa

revision = '001'
down_revision = None
branch_labels = None
depends_on = None

def upgrade():
    op.create_table(
        'products',
        sa.Column('id', sa.Integer(), nullable=False, primary_
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

