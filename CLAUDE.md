# Trust, Not Capability

This repo publishes a prompt system for auditing repository history and producing better CLAUDE.md files. The content is markdown, not code. There is no build step.

## Writing conventions

No promotional language. No jargon. Plain, direct descriptions.

Do not use emojis in any content.

The tone is "quiet authority": competent, clear, unpretentious. Write as if addressing a senior engineer who is busy and skeptical.

## Structure

`skills/deep-init/SKILL.md` is the core product — the full audit specification. This file is both the readable specification and the Claude Code skill. Changes here affect what every user gets when they run `/deep-init`.

Content is published at the repo root, not inside `.claude/`. The `.claude/` directory is the consumer's directory — when someone copies `skills/deep-init/` into their repo, it goes into their `.claude/skills/`. This repo publishes content; it does not consume it. This follows the same pattern as `anthropics/skills`.

## Where new content goes

| New content | Location |
|---|---|
| New skill (e.g. `/add-correction`) | `skills/add-correction/SKILL.md` |
| Separated subagent prompts (when adapters are needed) | `agents/github-pr-mining.md` (new directory) |
| Hook scripts | `hooks/pre-commit.sh` (new directory) |
| Starter CLAUDE.md template | `templates/starter-claudemd.md` (new directory) |
| Companion article (if hosted in-repo) | `docs/designing-a-prompt-like-a-system.md` (new directory) |

Each addition is a new file in an existing directory, or a new directory at the root level with an obvious name. No existing files need to move.

## Maintaining the audit prompt

The SKILL.md is a single self-contained specification. Subagent instructions are inline sections, not separate files.

When editing the prompt, preserve the five-step structure (review existing, mine history, synthesize, generate, output) and the two-phase execution approach (git reconnaissance first, then three parallel extractors).

The content categories checklist and structural principles sections define what the audit produces. Changes there affect the output format.

## What not to do

Do not split the subagent sections into separate files unless adapters for multiple source platforms (GitHub, GitLab, Linear, etc.) are being actively maintained. Premature abstraction.

Do not add empty placeholder files or directories for future content. Directories appear when there is content to put in them.

Do not duplicate content between the README and the companion article. The README links to the article; it does not summarize it.
