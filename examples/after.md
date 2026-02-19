# Example: /deep-init output (after audit)

This is a sanitised example of what the audit produces for the same repository. The domain rules, edge cases, and coding patterns were extracted from PR comments, Jira tickets, and Confluence pages — sources that `/init` does not read.

<!-- TODO: Replace with real sanitised example from a production repo -->

```markdown
# nav-calculator
Last audited: 2026-02-19

Portfolio NAV calculation engine for the Multi-Strategy fund. Errors here produce
incorrect fund valuations reported to investors and regulators. A wrong NAV
triggers a restatement process that takes 3-5 business days.

## Domain rules

FX forwards use T-0 rates from the live feed, not T-1 close prices.
All other asset classes use T-1. This is because FX forwards settle on
trade date, unlike most instruments.

Never call the pricing service directly during the NAV window (16:00-17:30 UTC).
Use the cached snapshot from `PriceCache.get_nav_snapshot()`.

Hedging overlay calculations run after the base NAV, not before.
The overlay adjusts the base; reversing the order double-counts hedges.
See `apply_hedge_overlay` in src/nav/calculator.py.

Exclude pending settlements until T+2.
See `filter_settlements` in src/nav/settlement_filter.py.

When a position has no price on the valuation date, use the last available
price with a staleness flag, not zero. Zero prices cascade into incorrect
P&L attribution.

Mandate-level NAV must agree with fund-level NAV after aggregation.
If they diverge, the mandate allocations are wrong, not the fund NAV.

## Build and test

`pytest` runs the full suite. No special flags.
Pre-commit hooks enforce ruff and mypy automatically.

`pytest tests/nav/ -m "not slow"` for a quick check of NAV logic only.

## Coding patterns

Use Arrow-backed DataFrames (`dtype_backend="pyarrow"`), not the default
NumPy backend. See `load_positions` in src/data/loader.py.

Date handling uses `pandas.Timestamp`, not `datetime.date`. The codebase
relies on timezone-aware timestamps throughout. Do not introduce naive
datetimes.

Price lookups go through `PriceCache`, never directly to the pricing API.
The cache handles staleness checks, fallback logic, and NAV window restrictions.

## Known mistakes

[Empty at launch. Add entries here via the # key when Claude gets something
wrong during development.]

## References

Before modifying anything in src/nav/, read docs/claude/domain-rules.md.
Before modifying src/data/loader.py, read docs/claude/edge-cases.md.
For business term definitions (NAV, T+2, mandate, overlay), see docs/claude/domain-glossary.md.
For system architecture and data flow, see docs/claude/architecture.md.
```

Note what the audit added: six domain rules (all extracted from PR comments and Jira bug tickets), specific coding patterns with authoritative code references, and conditional pointers to reference files for detailed context. The instruction count is 18 — well within the budget of ~100.
