# Example: /init output (before audit)

This is a sanitised example of what `/init` produces for a typical Python repository. The output is accurate but generic — it describes what the code does, not what the team knows.

<!-- TODO: Replace with real sanitised example from a production repo -->

```markdown
# nav-calculator

## Build and test

Run tests with pytest:
    pytest

Lint with ruff:
    ruff check src/ tests/

Type checking with mypy:
    mypy src/

## Project structure

- src/nav/ — Core NAV calculation modules
- src/data/ — Data loading and transformation pipelines
- src/pricing/ — Price feed integrations
- src/reporting/ — Report generation
- tests/ — Test suite organised by module
- config/ — Configuration files

## Key files

- src/nav/calculator.py — Main NAV calculation engine
- src/nav/fx_pricing.py — FX rate lookups and forward pricing
- src/data/loader.py — Position and trade data loading
- src/pricing/cache.py — Price caching layer

## Coding conventions

- Use type hints for all public function signatures
- Follow PEP 8 naming conventions
- Use pathlib.Path instead of os.path for file operations
- Prefer list comprehensions over map/filter
- All modules should have docstrings

## Common development tasks

- To add a new asset class, create a new module in src/nav/
- To modify pricing logic, update the relevant pricing module
- Run the full test suite before submitting PRs
```

Note what is absent: no domain rules, no edge cases, no explanation of why FX forwards are different from other asset classes, no mention of the NAV window timing constraint, no warning about settlement date handling. These are the rules that cost money when violated, and `/init` cannot discover them because they live in the team's experience, not in the code structure.
