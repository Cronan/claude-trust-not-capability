# Trust, Not Capability

This repo publishes Claude Code skills as prompt specifications. The core skill audits repository history and produces a CLAUDE.md grounded in what the team actually learned, not just what the code looks like today.

## Skills

### `/deep-init` — audit repo history and rebuild CLAUDE.md

Claude Code's `/init` produces a CLAUDE.md by reading the code. That is necessary but insufficient. The team's real knowledge — domain rules, edge cases, the mistakes that cost money — lives in PR comments, issue tracker tickets, and commit history. `/deep-init` mines those sources and produces a CLAUDE.md grounded in what the team actually learned.

Full specification: [skills/deep-init/SKILL.md](skills/deep-init/SKILL.md)

### `/deglazer` — remove AI writing patterns from text

A writing editor skill that identifies and corrects signs of AI-generated text. Covers surface patterns (vocabulary, structure, formatting), deep patterns (epistemic uniformity, register lock, causal over-assertion), and corpus-level patterns (cross-text repetition, structural homogeneity). Organized by detection severity into three tiers.

Full specification: [skills/deglazer/SKILL.md](skills/deglazer/SKILL.md)

## Quick start

Copy any skill directory into your repo's `.claude/skills/` directory and invoke it by name in Claude Code.

```
cp -r skills/deep-init/ your-repo/.claude/skills/deep-init/
```

For `/deep-init`: your repo needs an existing CLAUDE.md from `/init`. The audit builds on `/init` output (build commands, project structure), not replaces it. It reads but does not write — it outputs the proposed CLAUDE.md for human review.

## `/deep-init` in detail

### Before and after

<details>
<summary><strong>/init output</strong> (generic, code-derived)</summary>

```markdown
# MyRepo

## Build and test
Run `pytest` to execute tests. Run `ruff check` for linting.

## Project structure
- src/orders/ — Order processing modules
- src/inventory/ — Stock management and allocation
- tests/ — Test suite

## Coding conventions
- Use type hints for all function signatures
- Follow PEP 8 naming conventions
```

</details>

<details>
<summary><strong>After /deep-init</strong> (history-derived, domain-specific)</summary>

```markdown
# MyRepo
Last audited: 2026-02-19

Order processing and fulfillment engine. Errors here produce incorrect charges
or oversold inventory.

## Domain rules

Stock reservations expire after 30 minutes if payment is not captured.
Never extend the window; it causes overselling during flash sales.

Never query the warehouse API during the nightly inventory sync (02:00-03:30 UTC).
Use the cached snapshot from `InventoryCache.get_snapshot()`.

Discount application runs after stock validation, not before.
Applying discounts to out-of-stock items generates refund obligations
for orders that should have been rejected.

Exclude pre-orders from available stock counts until the goods-in receipt
is confirmed. See `filter_preorders` in src/inventory/preorder_filter.py.

## Build and test

`pytest` runs the full suite. No special flags needed.
Pre-commit hooks enforce ruff and mypy — do not skip them.

## Coding patterns

Use Arrow-backed DataFrames (`dtype_backend="pyarrow"`), not the default NumPy backend.
See `load_orders` in src/data/loader.py for the canonical pattern.

## References

Before modifying anything in src/orders/, read docs/claude/domain-rules.md.
For business term definitions, see docs/claude/domain-glossary.md.
```

</details>

### What the audit does

The audit mines four sources in two phases:

**Phase 1 — Reconnaissance.** A git history subagent scans the log to identify hot files (files with recurring fix commits), revert commits, and ticket IDs. This runs first because its output focuses the other three subagents.

**Phase 2 — Domain extraction.** Three subagents run in parallel:

- **PR comments**: Extracts domain rules from reviewer feedback on merged PRs, prioritizing PRs that touch hot files
- **Issue tracker tickets**: Reads closed bugs, stories, and tasks for root cause analysis and acceptance criteria with specific domain constraints
- **Team wiki pages**: Searches for ADRs, glossaries, and runbooks (the three page types with signal)

**Synthesis.** The orchestrating agent cross-references findings by ticket ID, clusters duplicates, assigns scope from git data, applies a rule test (does this reveal something Claude cannot discover from the code?), and writes concise instructions.

### Adapting for your stack

The audit uses generic terms — pull requests, issue tracker, team wiki — rather than naming specific tools. The three domain extraction subagents each have their own heading in the SKILL.md, making them straightforward to adjust.

Claude Code needs access to your team's tools to run the extraction. This typically means MCP servers or API integrations for your PR platform, issue tracker, and documentation wiki. The git subagent works with any git repository directly.

The interface each subagent must satisfy:

- **Input**: receives the hot files list and ticket IDs from Phase 1
- **Output**: candidate findings, each with what the source says, the source reference, files touched, and any ticket IDs

The orchestration logic (Phase 1, synthesis, CLAUDE.md generation) is source-agnostic.

### Limitations

- The subagents are prompts running inside Claude Code, not standalone scripts or packaged tools.
- Quality depends on the target repo's review culture. Repos with sparse PR comments produce sparse results.
- Designed for English-language sources.
- The audit covers 6 months of commits and PRs, 12 months of issue tracker tickets. Repos with low activity or key decisions made long ago may need the time windows extended.

## The thinking behind this

This repo is the concrete artifact of a broader project connecting classical software engineering literature to frontier AI lab practices. The series covers Brooks (*The Mythical Man-Month*), Conway's Law, Deming's PDSA cycle, and DORA/Accelerate — applied to the question of how teams should work with AI coding agents.

The thesis: AI is a force multiplier only if systems are designed to absorb mistakes cheaply. CLAUDE.md is Deming's PDSA cycle in a markdown file. Every mistake the team catches becomes a rule. The audit command produces the initial rules; the `#` key maintains them over time.

## Contributing

Suggestions and improvements welcome via [issues](../../issues).

The subagent sections in `skills/deep-init/SKILL.md` are written generically. If you find adjustments that work well for a specific platform, consider submitting a PR.

## License

MIT. See [LICENSE](LICENSE).
