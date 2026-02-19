# Repository Structure Proposal

## Research summary

I studied three categories of repos to inform this design.

**Prompt and agent repos** (Anthropic's `skills`, `claude-code` plugins, `claude-cookbooks`; OpenAI's `openai-agents-python`; CrewAI; LangChain Hub). The consistent pattern: one directory per skill or agent, progressive disclosure (light metadata loaded at startup, full instructions on demand), and content at the repo root for publishing rather than inside `.claude/` for consumption. Anthropic's `skills` repo publishes skills in a top-level `skills/` directory. Claude Code plugins bundle skills, agents, and commands inside plugin directories.

**Claude Code conventions** (official documentation, community guides). Claude Code discovers skills at `.claude/skills/<name>/SKILL.md`, agents at `.claude/agents/<name>.md`, and commands at `.claude/commands/<name>.md`. Skills are the recommended approach over legacy commands. Skills use `SKILL.md` with YAML frontmatter, support `references/` for heavy documentation loaded on demand, and `scripts/` for executable code. Only the frontmatter (name + description) is loaded at session startup; the full body loads when invoked.

**Developer tool repos** (commitizen, semantic-release, husky, lint-staged, danger-js, changesets, release-it). Two patterns dominate. First, radical simplicity: husky is two files; lint-staged is a flat directory of 35 files with no subdirectories. Second, extensibility through clear interfaces rather than plugin systems. The strongest repos dogfood their own tools. README strategies split into two schools: everything in one long README (commitizen, lint-staged) or a brief landing page that funnels into `docs/` (semantic-release, husky, danger-js).

---

## Proposed structure

```
.
├── README.md
├── LICENCE
├── .gitignore
├── CLAUDE.md
│
├── skills/
│   └── deep-init/
│       └── SKILL.md
│
└── examples/
    ├── before.md
    └── after.md
```

Five content files plus three infrastructure files (README, LICENCE, .gitignore). One level of nesting for most content; two levels for the skill (required by the SKILL.md convention).

The skill is named `deep-init` — a deliberate callback to Claude Code's `/init` command. `/init` reads the code; `/deep-init` reads the history. The name signals the relationship: run `/init` first, then `/deep-init` to go deeper.

### Directory rationale

**`skills/deep-init/SKILL.md`** — The core product. The complete audit specification in a single file: the five-step workflow, the two-phase execution approach with four subagent definitions, the content categories checklist, and the structural principles for CLAUDE.md generation.

A user copies `skills/deep-init/` into their own `.claude/skills/` directory and invokes it with `/deep-init`. This follows the same publishing pattern as Anthropic's `anthropics/skills` repository: skills live at the repo root for browsing and copying, not inside `.claude/` (which is reserved for the repo's own development configuration).

The subagent instructions (git reconnaissance, PR mining, Jira analysis, Confluence extraction) live inline within the SKILL.md rather than as separate files. The prompt was written as one coherent specification — steps reference each other, the synthesis in Step 3 depends on the output formats defined in Step 2, and the content categories in Step 4 reference the findings format. Splitting the subagents into separate files would add indirection without adding clarity, and would force readers to jump between files to understand the workflow.

If the repo later needs to support a matrix of source adapters (GitHub + Bitbucket + GitLab for PRs; Jira + Linear + Shortcut for tickets), that is the point at which extracting agents into separate files becomes justified. The refactor is a one-commit change when the need is real.

*How a user interacts with it:* Copy the directory into `.claude/skills/` in their target repo. Type `/deep-init` in Claude Code. Read the SKILL.md directly on GitHub to understand the methodology.

**`examples/`** — Sanitised before/after showing what the audit produces. `before.md` is a typical `/init` output. `after.md` is the same repo's CLAUDE.md after running the audit. This is the proof that the tool works. It belongs in the README's first screen and in conference presentations.

*How a user interacts with it:* Read them on GitHub to decide whether the tool is worth trying. Reference them when explaining the approach to their team.

**`CLAUDE.md`** — Instructions for developing this repo itself. Not the product; the development configuration. Covers style conventions for the writing and how to maintain the prompt specification. A quiet dogfooding: a repo about building better CLAUDE.md files should have a good one of its own.

### Where future content goes

The structure handles growth without reorganisation:

| New content | Where it goes |
|---|---|
| New skill (e.g. `/add-correction`) | `skills/add-correction/SKILL.md` |
| Separated subagent prompts (when adapters are needed) | `agents/github-pr-mining.md` (new directory) |
| Hook scripts | `hooks/pre-commit.sh` (new directory) |
| Starter CLAUDE.md template | `templates/starter-claudemd.md` (new directory) |
| Companion article (if hosted in-repo) | `docs/designing-a-prompt-like-a-system.md` (new directory) |
| Additional examples | `examples/python-monorepo.md` |

Each addition is a new file in an existing directory, or a new directory at the root level with an obvious name. No existing files need to move.

### What is NOT in `.claude/`

The `.claude/` directory is deliberately absent from the published structure.

1. `.claude/` is the consumer's directory. When someone copies `skills/deep-init/` into their repo, it goes into _their_ `.claude/skills/`. This repo publishes content; it does not consume it.

2. If this repo needed its own Claude Code configuration (hooks, settings, permissions), those would go in `.claude/settings.json` and would be about developing the repo, not about the audit tool.

3. This matches the `anthropics/skills` pattern: publishing repos keep content at the root level; consuming repos put it in `.claude/`.

---

## README design

The README has two audiences: practitioners (want to use the tool) and engineering leaders (want to understand the thinking). Practitioners need speed. Leaders need seriousness. The structure serves both without making either wait.

### Sections

**1. Opening paragraph** (the problem, not the tool)

Three sentences. The problem: `/init` produces generic output because it only reads the code. The insight: the team's real knowledge — domain rules, edge cases, the mistakes that cost money — lives in PR comments, Jira tickets, and commit history. The tool: this repo provides a prompt system that mines those sources and produces a CLAUDE.md grounded in what the team actually learned.

No badges. No logo. No "awesome" list markers. The opening should read like the first paragraph of a technical paper.

**2. Before and after** (proof, not promise)

A side-by-side or sequential comparison: what `/init` produces vs what the audit produces for the same repo. Collapsed or truncated to fit in one screen. Link to `examples/` for the full versions. This answers the question "why should I bother?" without requiring the reader to install anything.

**3. Quick start** (three steps)

```
1. Copy skills/deep-init/ into your repo's .claude/skills/ directory
2. Run /deep-init in Claude Code
3. Review the output before committing
```

Each step is one line. No prerequisites beyond having Claude Code installed. Mention that the audit reads but does not write — it outputs the proposed CLAUDE.md for human review.

**4. What the audit does** (the method)

Brief description of the two-phase, four-source approach. Not a reproduction of the full specification — a summary that gives confidence the tool is systematic, not a toy:

- Phase 1: Git reconnaissance (identifies hot files, revert commits, Jira IDs)
- Phase 2: Three parallel extractors (PR comments, Jira tickets, Confluence pages)
- Synthesis: Cross-references findings, applies the rule test, writes instructions

Link to `skills/deep-init/SKILL.md` for the full specification.

**5. Adapting for your stack**

This section is necessary because the subagents currently assume Bitbucket, Jira, and Confluence. One paragraph explaining that the subagent instructions are clearly demarcated sections within the SKILL.md, designed to be modified for different sources. The interface each section must satisfy: what input it receives (hot files list, Jira IDs from git), what output format it must produce (candidate findings with source references). Point to the relevant sections by header name.

**6. Limitations**

Honest list:

- Currently written for Bitbucket, Jira, and Confluence (modifiable, but not out-of-the-box for GitHub/Linear/Notion)
- The subagents are prompts running inside Claude Code, not standalone tools or scripts
- Quality depends on the target repo's review culture — repos with sparse PR comments produce sparse results
- Designed for English-language sources
- Requires Claude Code with access to the target repo's git history and (optionally) API access to the ticket/PR/wiki systems

**7. The thinking behind this**

Two paragraphs connecting to the broader project. This repo is the concrete artifact of a five-part series on AI and software engineering practice. The series connects classical literature (Brooks, Conway, Deming, DORA) to frontier AI lab practices. The thesis: AI is a force multiplier only if systems are designed to absorb mistakes cheaply. CLAUDE.md is Deming's PDSA cycle in a markdown file.

Link to the companion article and Substack series when published. Do not reproduce the argument — let the reader choose their depth.

**8. Contributing**

Brief. How to suggest improvements (issues). How to adapt the prompt for different sources (fork and modify the relevant subagent section). Style note: British English, plain language, no promotional tone.

**9. Licence**

One line. MIT.

---

## Repo names

Five options, ordered by my recommendation.

### 1. `trust-not-capability` (recommended)

The project name. Clean, memorable, serious. Works well in an article or on a conference slide as part of the narrative: "the repo is at `trust-not-capability`." Signals that this is a considered framework, not a weekend hack. The repo may eventually contain more than just the audit prompt (hooks, templates, additional skills), and the project name accommodates that growth.

Risk: not discoverable by someone searching for CLAUDE.md tooling. Compensate with GitHub topics: `claude-md`, `claudemd`, `claude-code`, `audit`, `ai-coding-agents`.

### 2. `claude-md-audit` (recommended if leading with the tool)

Discoverable. Someone searching GitHub for "CLAUDE.md" or "claude md" will find it. Describes what the tool does. Professional. Works as a URL on a conference slide: `github.com/ivancronyn/claude-md-audit`. Short enough to type.

Risk: sounds like it audits the CLAUDE.md file itself, rather than producing one from history. Mitigated by the README's opening paragraph. Also limits the repo's identity to one tool if the project grows.

### 3. `claudemd-audit`

Variant of option 2 without the hyphen between "claude" and "md". Slightly less readable but marginally shorter. Treats "claudemd" as a compound noun.

### 4. `repo-audit-claudemd`

Fully descriptive. Makes clear that the audit target is the repo (its history, PRs, tickets), and the output is a CLAUDE.md. Longer than ideal for slides but unambiguous.

### 5. `claude-md-forge`

"Forge" implies crafting something from raw materials (git history, PR comments). More evocative than "audit" but risks sounding clever rather than serious.

### Current name: `claude-trust-not-capability`

Combines the tool context (claude) with the project name (trust-not-capability). The longest option. If you want a single name that does both, this works. If you want something tighter, options 1 or 2 are better depending on whether you lead with the tool or the thesis.

### My recommendation

Use **`trust-not-capability`**. The thesis is the durable asset; the tool is one expression of it. Add discoverability through GitHub topics rather than cramming everything into the repo name.

---

## Design decisions and trade-offs

### Single SKILL.md vs separated subagent files

The audit prompt stays as one file. The subagent instructions are inline sections, not separate files. Reasons:

1. **The spec reads as one document.** Steps reference each other. Step 3's synthesis depends on Step 2's output formats. Breaking it apart adds indirection without adding clarity.
2. **Claude Code doesn't need separate files to spawn subagents.** The SKILL.md tells Claude how to use the Task tool with inline instructions. Separate agent files would be an additional layer of indirection.
3. **Adapting for different sources is a text edit, not a file swap.** The sections are clearly labelled with headers. Fork, find the "Bitbucket PRs" section, replace with "GitHub PRs." Simpler than understanding an agent interface contract across files.
4. **Premature abstraction.** The repo's own design principles warn against it. If adapter combinations proliferate, extract then. The refactor is straightforward.

### `skills/` at root vs inside `.claude/`

Publishing repos keep content at the root level. `.claude/` is for consumption. This follows `anthropic/skills`. The trade-off: you cannot clone and immediately type `/deep-init` to test. You must copy the skill into `.claude/` first. Clarity of purpose over instant dogfooding.

### No `src/` directory

The product is markdown, not code. No build step, no compilation, no packages. The content lives at the top level where it is immediately visible.

### Companion article lives externally

The companion article publishes on Substack and the author's website first. The README links to it there. If a copy is later wanted in the repo, `docs/` appears at that point. No empty directory in the meantime.

### British English throughout

Deliberate stylistic choice. The LICENCE file uses the British spelling. All content uses British conventions. This signals the author's identity and the project's consistency.

### No empty directories at launch

Directories appear when there is content to put in them. `docs/`, `hooks/`, `templates/`, and `agents/` are documented as future expansion points but not created until needed.
