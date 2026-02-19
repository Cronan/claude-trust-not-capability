# Trust, Not Capability

Claude Code's `/init` command produces a CLAUDE.md by reading the code. That is necessary but insufficient. The team's real knowledge — domain rules, edge cases, the mistakes that cost money — lives in PR comments, Jira tickets, and commit history. This repo provides a prompt system that mines those sources and produces a CLAUDE.md grounded in what the team actually learned.

## Before and after

<!-- TODO: Replace with real sanitised examples -->

<details>
<summary><strong>/init output</strong> (generic, code-derived)</summary>

```markdown
# MyRepo

## Build and test
Run `pytest` to execute tests. Run `ruff check` for linting.

## Project structure
- src/nav/ — NAV calculation modules
- src/data/ — Data loading and transformation
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

Portfolio NAV calculation engine. Errors here produce incorrect fund valuations
reported to investors and regulators.

## Domain rules

FX forwards use T-0 rates from the live feed, not T-1 close prices.
All other asset classes use T-1.

Never call the pricing service during the NAV window (16:00-17:30 UTC).
Use the cached snapshot from `PriceCache.get_nav_snapshot()`.

Hedging overlay calculations run after the base NAV, not before.
The overlay adjusts the base; reversing the order double-counts hedges.

Exclude pending settlements until T+2.
See `filter_settlements` in src/nav/settlement_filter.py.

## Build and test

`pytest` runs the full suite. No special flags needed.
Pre-commit hooks enforce ruff and mypy — do not skip them.

## Coding patterns

Use Arrow-backed DataFrames (`dtype_backend="pyarrow"`), not the default NumPy backend.
See `load_positions` in src/data/loader.py for the canonical pattern.

## References

Before modifying anything in src/nav/, read docs/claude/domain-rules.md.
For business term definitions, see docs/claude/domain-glossary.md.
```

</details>

Full examples in [examples/](examples/).

## Quick start

```
1. Copy skills/deep-init/ into your repo's .claude/skills/ directory
2. Run /deep-init in Claude Code
3. Review the output before committing
```

The audit reads but does not write. It outputs the proposed CLAUDE.md for human review.

**Prerequisite:** Your repo needs an existing CLAUDE.md from `/init`. The audit builds on `/init` output (build commands, project structure), not replaces it.

## What the audit does

The audit mines four sources in two phases:

**Phase 1 — Reconnaissance.** A git history subagent scans the log to identify hot files (files with recurring fix commits), revert commits, and Jira IDs. This runs first because its output focuses the other three subagents.

**Phase 2 — Domain extraction.** Three subagents run in parallel:

- **PR comments**: Extracts domain rules from reviewer feedback on merged PRs, prioritising PRs that touch hot files
- **Jira tickets**: Reads closed bugs, stories, and tasks for root cause analysis and acceptance criteria with specific domain constraints
- **Confluence pages**: Searches for ADRs, glossaries, and runbooks (the three page types with signal)

**Synthesis.** The orchestrating agent cross-references findings by Jira ID, clusters duplicates, assigns scope from git data, applies a rule test (does this reveal something Claude cannot discover from the code?), and writes concise instructions.

Full specification: [skills/deep-init/SKILL.md](skills/deep-init/SKILL.md)

## Adapting for your stack

The audit currently assumes Bitbucket for PRs, Jira for tickets, and Confluence for documentation. These are clearly demarcated sections within the SKILL.md, each with its own heading.

To adapt for GitHub, Linear, Notion, or other tools: fork this repo and modify the relevant subagent section. The interface each section must satisfy:

- **Input**: receives the hot files list and Jira IDs from Phase 1
- **Output**: candidate findings, each with what the source says, the source reference, files touched, and any ticket IDs

The orchestration logic (Phase 1, synthesis, CLAUDE.md generation) is source-agnostic.

## Limitations

- Written for Bitbucket, Jira, and Confluence. Adaptable but not out-of-the-box for GitHub, Linear, or Notion.
- The subagents are prompts running inside Claude Code, not standalone scripts or packaged tools.
- Quality depends on the target repo's review culture. Repos with sparse PR comments produce sparse results.
- Designed for English-language sources.
- The audit covers 6 months of commits and PRs, 12 months of Jira tickets. Repos with low activity or key decisions made long ago may need the time windows extended.

## The thinking behind this

This repo is the concrete artifact of a broader project connecting classical software engineering literature to frontier AI lab practices. The series covers Brooks (*The Mythical Man-Month*), Conway's Law, Deming's PDSA cycle, and DORA/Accelerate — applied to the question of how teams should work with AI coding agents.

The thesis: AI is a force multiplier only if systems are designed to absorb mistakes cheaply. CLAUDE.md is Deming's PDSA cycle in a markdown file. Every mistake the team catches becomes a rule. The audit command produces the initial rules; the `#` key maintains them over time.

<!-- TODO: Add links to companion article and Substack series when published -->

## Contributing

Suggestions and improvements welcome via [issues](../../issues).

To adapt the prompt for different sources (GitHub PRs, Linear tickets, Notion pages), fork the repo and modify the relevant subagent section in `skills/deep-init/SKILL.md`. If you build an adapter that works well, consider submitting a PR.

Style: British English, plain language, no promotional tone.

## Licence

MIT. See [LICENCE](LICENCE).
