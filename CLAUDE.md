# Trust, Not Capability

This repo publishes a prompt system for auditing repository history and producing better CLAUDE.md files. The content is markdown, not code. There is no build step.

## Writing conventions

British English throughout. No promotional language. No jargon. Plain, direct descriptions.

Do not use emojis in any content.

The tone is "quiet authority": competent, clear, unpretentious. Write as if addressing a senior engineer who is busy and sceptical.

## Structure

`skills/deep-init/SKILL.md` is the core product — the full audit specification. This file is both the readable specification and the Claude Code skill. Changes here affect what every user gets when they run `/deep-init`.

`examples/` contains sanitised before/after CLAUDE.md files. These must not contain real company data, team names, or proprietary domain rules.

`docs/` contains the companion article and any future writing.

## Maintaining the audit prompt

The SKILL.md is a single self-contained specification. Subagent instructions are inline sections, not separate files.

When editing the prompt, preserve the five-step structure (review existing, mine history, synthesise, generate, output) and the two-phase execution approach (git reconnaissance first, then three parallel extractors).

The content categories checklist and structural principles sections define what the audit produces. Changes there affect the output format.

## What not to do

Do not split the subagent sections into separate files unless adapters for multiple source platforms (GitHub, GitLab, Linear, etc.) are being actively maintained. Premature abstraction.

Do not add empty placeholder files or directories for future content. Directories appear when there is content to put in them.

Do not duplicate content between the README and the companion article. The README links to the article; it does not summarise it.
