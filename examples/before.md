# Example: /init output (before audit)

This is a sanitised example of what `/init` produces for a typical Python repository. The output is accurate but generic — it describes what the code does, not what the team knows.

```markdown
# order-engine

## Build and test

Run tests with pytest:
    pytest

Lint with ruff:
    ruff check src/ tests/

Type checking with mypy:
    mypy src/

## Project structure

- src/orders/ — Core order processing modules
- src/inventory/ — Stock management and warehouse allocation
- src/pricing/ — Discount and tax calculation
- src/fulfilment/ — Shipping and dispatch logic
- tests/ — Test suite organised by module
- config/ — Configuration files

## Key files

- src/orders/processor.py — Main order processing engine
- src/inventory/reservation.py — Stock reservation and expiry
- src/inventory/warehouse.py — Warehouse allocation logic
- src/pricing/discounts.py — Discount application pipeline

## Coding conventions

- Use type hints for all public function signatures
- Follow PEP 8 naming conventions
- Use pathlib.Path instead of os.path for file operations
- Prefer list comprehensions over map/filter
- All modules should have docstrings

## Common development tasks

- To add a new payment method, create a new module in src/payments/
- To modify discount logic, update the relevant pricing module
- Run the full test suite before submitting PRs
```

Note what is absent: no domain rules, no edge cases, no explanation of why stock reservations have a time limit, no mention of the inventory sync window, no warning about pre-order handling. These are the rules that cost money when violated, and `/init` cannot discover them because they live in the team's experience, not in the code structure.
