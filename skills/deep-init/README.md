# deep-init

Audits a repository's git history, PR comments, issue tracker tickets, and team wiki pages to produce a CLAUDE.md grounded in the team's actual experience.

## Why this exists

Claude Code's `/init` produces a CLAUDE.md by reading the code. That captures build commands, project structure, and conventions visible in the source. It misses the team's accumulated knowledge: domain rules discovered through bug fixes, edge cases documented in ticket root cause analyses, and design decisions recorded in PR reviews.

This skill mines those sources and produces a CLAUDE.md that encodes what the team learned, not just what the code looks like.

## Design rationale

### Instruction budget

Research on instruction-following (IFScale, July 2025) found that frontier thinking models follow roughly 150-200 instructions with reasonable consistency. Compliance degrades uniformly as instruction count increases. Claude Code's system prompt already contains approximately 50 instructions, leaving a budget of roughly 100-150 for the CLAUDE.md.

This is why the skill targets under 100 instructions in the root file and uses reference files for detail. It is not an arbitrary limit.

### Progressive disclosure

The Claude Code harness wraps CLAUDE.md content with a system-level instruction telling the model that the context may or may not be relevant and should be ignored if not highly relevant to the current task. This means entries about stock reservation rules may be filtered out when Claude is editing a logging utility in the same repo.

Two consequences: (1) the reference file approach matters because root file entries about a specific subdomain can get filtered during unrelated work; (2) pointers to reference files should be conditional, tied to file paths or code areas, so they appear relevant when Claude works in that area.

### File length

There is no magic line count. Anthropic says "concise" without specifying a number. HumanLayer keeps their root file under 60 lines. The real test: if a developer wouldn't read the whole file in one sitting, it's too long.

### Maintenance

The audit produces the initial file. The `#` key is how it stays alive. When a PR review catches an error, the correction goes into the relevant section via `#` before the PR merges. This is Deming's PDSA cycle encoded in a markdown file: every mistake becomes a rule.
