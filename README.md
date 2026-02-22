# claude-code-skills

Claude Code skills. Copy a skill directory into `.claude/skills/` in your repo and invoke it by name.

| Skill | What it does |
|---|---|
| [`/stet`](#stet) | Strip AI writing patterns from text |
| [`/deep-init`](#deep-init) | Audit repo history, rebuild CLAUDE.md |

## Installation

```
cp -r skills/stet/ your-repo/.claude/skills/stet/
```

Then type `/stet` in Claude Code.

---

## /stet

*Stet* is a proofreading mark from the Latin for "let it stand." An editor crosses something out, reconsiders, writes *stet* in the margin and dots the deletion -- the original stays.

This skill does the inverse. AI has overwritten your voice with its patterns: uniform sentence lengths, the same three-paragraph cadence, "furthermore" and "leverage" and "comprehensive" on every page. `/stet` identifies 27 of these patterns and strips them, letting what you actually meant to say come through.

### The 27 patterns

Organized by detection risk -- how likely they are to flag the text as AI-generated, whether by automated classifiers or attentive human readers.

**High detection risk** (structural tells that classifiers catch):

1. Sentence length uniformity -- every sentence is 15-25 words
2. Paragraph length uniformity -- every paragraph is 3-5 sentences
3. Epistemic uniformity -- equal confidence in every claim
4. Register consistency -- formality never shifts
5. Punctuation distribution -- only commas, periods, and dashes

**Medium detection risk** (patterns attentive readers catch):

6. Overused AI vocabulary -- furthermore, comprehensive, leverage, foster, delve, seamless, nuanced ...
7. Copula avoidance -- "serves as" instead of "is"
8. Significance inflation -- everything is pivotal, crucial, transformative
9. Superficial participle phrases -- "highlighting the importance of"
10. Causal over-assertion -- clean because/therefore chains where reality is messy
11. Vague attributions -- "industry experts suggest"

**Polish** (individually minor, collectively diagnostic):

12. Promotional language -- groundbreaking, revolutionary, game-changing
13. Formulaic section headers -- "Why this matters," "Looking ahead"
14. Weak transitions -- Furthermore, Moreover, In addition
15. Bureaucratic bloat -- "in order to," "due to the fact that"
16. Intensifiers -- very, really, extremely, incredibly
17. Formatting tells -- em dash overuse, excessive boldface
18. Communication artifacts -- "I hope this helps," "Let me know if"
19. Hedging and sycophancy -- "Great question!", "The possibilities are endless"

**Corpus-level** (visible only when comparing multiple texts by the same "author"):

20. Cross-text verbatim repetition
21. Structural homogeneity
22. Convergent framing devices
23. Motif overuse

**Voice and personality** (what to add, not just what to remove):

24. Emotional flatness -- no reaction to the material
25. Opinion absence -- every side presented with equal weight
26. Absence of specific detail -- "a large-scale system" vs. the actual system
27. Temporal flattening -- everything in vague present tense

### Before and after

**Before:**

> The team evaluated several deployment strategies before selecting blue-green deployments. This approach allows seamless switching between production environments. The rollback process becomes significantly simpler with this architecture. Teams can verify new releases against production traffic before committing fully.

**After:**

> The team tried three deployment strategies. Blue-green won, mostly because rollbacks are instant -- you just flip the route back. Whether the added infrastructure complexity is worth it depends on how often you actually need to roll back, which in our case turned out to be roughly once a sprint, so yes.

Each pattern includes diagnostics, before/after examples, and fix guidance. Full specification: [`skills/stet/SKILL.md`](skills/stet/SKILL.md).

The pattern list draws on published research including [Linguistic Characteristics of AI-Generated Text (arXiv, 2025)](https://arxiv.org/pdf/2510.05136) and [The Disappearing Author (2025)](https://researchleap.com/the-disappearing-author-linguistic-and-cognitive-markers-of-ai-generated-communication/). Full references in [`skills/stet/README.md`](skills/stet/README.md).

---

## /deep-init

Claude Code's `/init` produces a CLAUDE.md by reading the code. That captures build commands and project structure. It misses everything the team learned the hard way: domain rules from bug fixes, edge cases from ticket post-mortems, design decisions buried in PR reviews.

`/deep-init` mines git history, PR comments, issue tracker tickets, and team wiki pages, then produces a CLAUDE.md grounded in what the team actually learned.

### Before and after

<details>
<summary><strong>/init output</strong> (code-derived)</summary>

```markdown
# MyRepo

## Build and test
Run `pytest` to execute tests. Run `ruff check` for linting.

## Project structure
- src/orders/ -- Order processing modules
- src/inventory/ -- Stock management and allocation

## Coding conventions
- Use type hints for all function signatures
- Follow PEP 8 naming conventions
```

</details>

<details>
<summary><strong>After /deep-init</strong> (history-derived)</summary>

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

## Build and test

`pytest` runs the full suite. No special flags needed.
Pre-commit hooks enforce ruff and mypy -- do not skip them.

## Coding patterns

Use Arrow-backed DataFrames (`dtype_backend="pyarrow"`), not the default NumPy backend.
See `load_orders` in src/data/loader.py for the canonical pattern.

## References

Before modifying anything in src/orders/, read docs/claude/domain-rules.md.
For business term definitions, see docs/claude/domain-glossary.md.
```

</details>

### How the audit works

**Phase 1 -- Reconnaissance.** A git history subagent scans the log to identify hot files (files with recurring fix commits), revert commits, and ticket IDs.

**Phase 2 -- Domain extraction.** Three subagents run in parallel, focused by Phase 1 output:

- **PR comments**: domain rules from reviewer feedback on merged PRs
- **Issue tracker**: root cause analysis and acceptance criteria from closed tickets
- **Team wiki**: ADRs, glossaries, and runbooks

**Synthesis.** Findings are cross-referenced by ticket ID, deduplicated, tested against a rule ("does this reveal something Claude cannot discover from the code?"), and written as concise instructions.

Prerequisites: a repo with an existing CLAUDE.md from `/init`, and MCP server access to your PR platform, issue tracker, and wiki. The git subagent works with any git repository directly.

Full specification: [`skills/deep-init/SKILL.md`](skills/deep-init/SKILL.md). Design rationale: [`skills/deep-init/README.md`](skills/deep-init/README.md).

---

## Contributing

Suggestions welcome via [issues](../../issues). The subagent sections in the skill specifications are written generically -- if you find adjustments that work well for a specific platform, consider a PR.

## License

MIT. See [LICENSE](LICENSE).
