# Measurement plan for /deep-init rollout

A step-by-step plan for baselining your current state, deploying /deep-init to pilot repos, and measuring whether the enriched CLAUDE.md files reduce domain errors in AI-assisted development.

Timeline: 12 weeks total. 1-2 weeks setup, 8-10 weeks operating, 1 week analysis.

---

## Phase 1: Select pilot repos

**When:** Week 0

**Goal:** Choose 2-3 repos to run /deep-init on, and 2-3 comparable repos to leave unchanged as controls.

### Steps

1. List repos where the team actively uses Claude Code. Exclude repos with fewer than 50 merged PRs in the last 6 months (not enough review data for the audit or the baseline).

2. From the remaining repos, pick pairs that are similar in: team size, review culture, domain complexity, and commit frequency. One repo in each pair gets /deep-init (treatment), the other stays on /init only (control).

3. Record for each repo:
   - Repo name
   - Team size (number of active contributors in last 3 months)
   - Merged PR count (last 6 months)
   - Current CLAUDE.md state: does it exist, how many lines, how many domain rules (probably zero)
   - Treatment or control

### Prompt: assess a repo's suitability

Run this in Claude Code inside each candidate repo:

```
Read the CLAUDE.md. Count the total instructions and categorise them:
domain rules, build commands, coding patterns, and generic advice that
any project would have. Then run `git shortlog -sn --since="6 months ago"`
and `git log --oneline --since="6 months ago" | wc -l` to report the
contributor count and commit volume. Summarise the repo's suitability
for a /deep-init pilot: does it have enough history, enough contributors,
and a sparse enough CLAUDE.md to show improvement?
```

---

## Phase 2: Capture baseline

**When:** Week 0-1

**Goal:** Measure the current domain error rate before /deep-init changes anything.

### Metric 1: PR domain correction rate

The primary signal. Count reviewer comments on merged PRs that explain a domain rule or correct a business logic mistake. Normalise as corrections per PR.

**What counts as a domain correction:** A reviewer comment that explains *why* something is wrong in business/domain terms, not just that it's wrong. "This needs to use the original purchase price, not the current price" is a domain correction. "Rename this variable" is not.

#### Prompt: classify PR comments

Run this against each pilot repo (treatment and control). Adjust the time window and PR platform as needed.

```
Review the last 100 merged PRs in this repo. For each PR, read the
reviewer comments. Classify each comment into one of four categories:

1. DOMAIN CORRECTION: the reviewer states or corrects a business rule,
   calculation, sequencing requirement, data source choice, or edge case.
   The comment explains WHY something is wrong in domain terms.
2. STYLE/PREFERENCE: naming, formatting, code structure preferences
   without domain reasoning.
3. APPROVAL/NEUTRAL: LGTM, +1, approved, minor acknowledgements.
4. OTHER: questions, discussion, CI/tooling comments.

Output a table with columns: PR number, comment count by category.
Then output the totals and the ratio: domain corrections per PR.

Also flag any domain correction that appears on more than one PR
(same rule, different PRs). List these as "repeat corrections" with
the rule and the PR numbers where it appeared.
```

Record:
- Total domain corrections
- Domain corrections per PR (the rate)
- Number of repeat corrections (same rule on multiple PRs)
- List of repeated rules (these are the highest-value targets for /deep-init)

#### Prompt: shortened version for large PR volumes

If 100 PRs is too slow, sample instead:

```
Sample 30 merged PRs from the last 6 months, evenly spaced by date.
For each, classify reviewer comments as DOMAIN CORRECTION, STYLE,
APPROVAL, or OTHER. Report the domain correction rate per PR and
list any repeated domain corrections.
```

### Metric 2: Hot file revert rate

Run the git reconnaissance phase from /deep-init manually. This gives you the "current pain" numbers.

#### Prompt: git reconnaissance

```
Scan the git log for the last 6 months. Extract:

1. HOT FILES: files that appear in commits whose messages contain
   "fix", "revert", "correct", "wrong", "should be", "actually",
   "broken", or "bug". Rank by frequency. Report the top 15.

2. REVERT COMMITS: list all reverts with the original commit,
   files affected, and any ticket ID from the commit message.

3. TICKET IDS: extract every ticket ID (e.g. PROJ-123 pattern)
   from commit messages.

Report: total fix-related commits, total reverts, total unique
ticket IDs found, and the hot files ranked list.
```

Record:
- Number of fix-related commits
- Number of reverts
- The hot files list (you will compare this in 12 weeks)

### Metric 3: # key baseline

If the team is already using Claude Code's `#` key to add corrections, count current entries in each CLAUDE.md. If not, the baseline is zero. Either way, start tracking from this point.

Record:
- Number of existing `#` key entries (probably zero)
- Date tracking begins

---

## Phase 3: Deploy /deep-init

**When:** Week 1-2

**Goal:** Run the audit on treatment repos and get the enriched CLAUDE.md files reviewed and committed.

### Steps

1. Copy `skills/deep-init/` into each treatment repo's `.claude/skills/` directory.

2. Run `/deep-init` in Claude Code. The audit will output a proposed CLAUDE.md and reference files without committing anything.

3. Have the repo owner review the output. They should:
   - Confirm domain rules are accurate (not outdated or misinterpreted)
   - Remove any rules they disagree with
   - Add any rules the audit missed that they know from experience
   - Check that reference file pointers use the correct paths

4. Commit the reviewed CLAUDE.md and any reference files.

5. Record for each treatment repo:
   - Number of domain rules in the new CLAUDE.md
   - Number of rules the owner removed (audit was wrong)
   - Number of rules the owner added (audit missed)
   - Number of reference files created
   - Total instruction count

### Save the audit output

The raw findings from Step 2 of the audit are your baseline document. They tell you how many domain rules existed in the team's history that were not in the CLAUDE.md. Save these separately; you will reference them in Phase 5.

---

## Phase 4: Operate and maintain

**When:** Weeks 2-10

**Goal:** Let the team work normally with the enriched CLAUDE.md files. Track maintenance activity.

### What the team should do

- Use Claude Code as normal on both treatment and control repos.
- When a PR review catches Claude making a domain error, add the correction to the CLAUDE.md via `#` before merging. This applies to both treatment and control repos (the control repos get organic `#` key additions but not the /deep-init audit).
- Do not run /deep-init on control repos during the measurement period.

### What to track weekly

A lightweight weekly check. Takes 5 minutes per repo.

#### Prompt: weekly check

Run this in each pilot repo:

```
Read the CLAUDE.md. Count the total instructions and the number
of entries in the "Known mistakes" section. Compare against last
week's count. Report any new entries added this week.
```

Record:
- New `#` key additions this week (count and content summary)
- Any domain errors caught in PR review this week that were NOT already in the CLAUDE.md

The second point matters: a domain error caught in review that is already covered by a CLAUDE.md rule means Claude ignored the instruction. A domain error not covered means the CLAUDE.md is incomplete.

---

## Phase 5: Measure and compare

**When:** Weeks 10-12

**Goal:** Re-run the baseline metrics and compare treatment vs control.

### Re-run Metric 1: PR domain correction rate

Use the same prompt from Phase 2, but covering PRs merged during weeks 2-10 only (the operating period).

```
Review merged PRs from [start date] to [end date] in this repo.
Classify reviewer comments as DOMAIN CORRECTION, STYLE, APPROVAL,
or OTHER. Report the domain correction rate per PR and list any
repeat corrections.
```

Compare:
- **Treatment repos:** domain correction rate (weeks 2-10) vs baseline (Phase 2)
- **Control repos:** domain correction rate (weeks 2-10) vs baseline (Phase 2)
- **Treatment vs control:** did the treatment repos improve more than the controls?

The key number: did the domain correction rate drop in treatment repos? Did repeat corrections decrease or disappear for rules that are now in the CLAUDE.md?

### Re-run Metric 2: Hot file revert rate

Use the same git reconnaissance prompt from Phase 2, scoped to the operating period.

Compare:
- Revert count: operating period vs baseline period
- Hot files list: did the same files keep appearing, or did the list shift?

### Analyse Metric 3: # key growth

Review the weekly tracking data from Phase 4.

For treatment repos:
- How many `#` key additions were made?
- Did the rate decline over time? (Expected: spike early, then decline)
- Were any additions for rules the audit should have caught?

For control repos:
- How many `#` key additions were made organically?
- What would the CLAUDE.md look like if someone had run /deep-init at the start?

### Compile results

#### Prompt: generate comparison report

```
I have baseline and post-measurement data for a /deep-init pilot.
Help me compile the comparison.

Treatment repos: [list repos]
Control repos: [list repos]
Operating period: [start] to [end]

Baseline data:
[paste Phase 2 numbers for each repo]

Post-measurement data:
[paste Phase 5 numbers for each repo]

Weekly # key tracking:
[paste Phase 4 weekly summaries]

Calculate:
1. Domain correction rate change (baseline vs operating) for each repo
2. Repeat correction change for treatment repos
3. Revert rate change for each repo
4. # key addition rate and trend for each repo

Summarise: did the /deep-init treatment repos show measurable
improvement relative to the control repos? What is the effect size?
Flag any confounds (team changes, major releases, repo activity
differences) that could explain the results.
```

---

## What good looks like

After 8-10 weeks of operation:

- **Domain correction rate drops 30-50% in treatment repos** relative to controls. The audit pre-loaded the most common corrections, so reviewers make them less often.
- **Repeat corrections approach zero** for rules in the CLAUDE.md. If the same rule keeps getting corrected despite being in the file, the rule is poorly written or the instruction budget is too high.
- **# key additions decline over time.** A sustained high rate means the audit missed important rules. A rate that drops to zero means the team stopped maintaining the file.
- **Control repos show no significant change.** This confirms the improvement is from /deep-init, not from other factors.

## What bad looks like

- **No difference between treatment and control.** The CLAUDE.md rules are being ignored (instruction budget exceeded? rules poorly written? hooks needed instead of advisory instructions?).
- **Domain correction rate increases in treatment repos.** The audit introduced incorrect rules that are causing new errors. Review the rules with the repo owner.
- **# key additions stay at zero throughout.** The team is not maintaining the file. The PDSA loop is broken. This is a process problem, not a tool problem.

---

## Deciding what to do next

If the pilot shows improvement: roll out /deep-init to remaining repos. Prioritise repos with high domain correction rates in the baseline (most room for improvement).

If the pilot shows no change: investigate whether the rules are being read (check instruction count against the ~100 budget), whether the rules are correctly written (ask the team to review), and whether the team is using `#` to maintain the file.

If the pilot shows mixed results: compare the treatment repos against each other. Repos with strong review cultures (more PR comments in the baseline) tend to produce better audits because there is more signal to extract.
