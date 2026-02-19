# Example: /deep-init output (after audit)

This is a sanitised example of what the audit produces for the same repository. The domain rules, edge cases, and coding patterns were extracted from PR comments, issue tracker tickets, and wiki pages — sources that `/init` does not read.

```markdown
# order-engine
Last audited: 2026-02-19

Order processing and fulfilment engine for the main e-commerce platform.
Errors here produce incorrect charges, failed deliveries, or oversold inventory.
A stock discrepancy during a flash sale triggers a manual reconciliation that
takes 2-3 business days.

## Domain rules

Stock reservations expire after 30 minutes if payment is not captured.
Never extend the reservation window — it causes overselling during flash sales.

Never query the warehouse API during the nightly inventory sync (02:00-03:30 UTC).
Use the cached snapshot from `InventoryCache.get_snapshot()`.

Discount application runs after stock validation, not before. Applying discounts
to out-of-stock items generates refund obligations for orders that should have
been rejected. See `apply_discounts` in src/orders/pricing.py.

Exclude pre-orders from available stock counts until the goods-in receipt
is confirmed. See `filter_preorders` in src/inventory/preorder_filter.py.

When a SKU has no warehouse allocation on the fulfilment date, route to
the default warehouse with a manual-review flag, not reject the order.
Rejections cascade into customer service escalations.

Line-item totals must agree with the order total after discounts and tax.
If they diverge, the line-item calculations are wrong, not the order total.

## Build and test

`pytest` runs the full suite. No special flags.
Pre-commit hooks enforce ruff and mypy automatically.

`pytest tests/orders/ -m "not slow"` for a quick check of order logic only.

## Coding patterns

Use Arrow-backed DataFrames (`dtype_backend="pyarrow"`), not the default
NumPy backend. See `load_orders` in src/data/loader.py.

Date handling uses `pandas.Timestamp`, not `datetime.date`. The codebase
relies on timezone-aware timestamps throughout. Do not introduce naive
datetimes.

Stock lookups go through `InventoryCache`, never directly to the warehouse API.
The cache handles staleness checks, fallback logic, and sync window restrictions.

## Known mistakes

[Empty at launch. Add entries here via the # key when Claude gets something
wrong during development.]

## References

Before modifying anything in src/orders/, read docs/claude/domain-rules.md.
Before modifying src/inventory/reservation.py, read docs/claude/edge-cases.md.
For business term definitions (SKU, reservation, fulfilment, pre-order), see docs/claude/domain-glossary.md.
For system architecture and data flow, see docs/claude/architecture.md.
```

Note what the audit added: six domain rules (all extracted from PR comments and bug tickets), specific coding patterns with authoritative code references, and conditional pointers to reference files for detailed context. The instruction count is 18 — well within the budget of ~100.
