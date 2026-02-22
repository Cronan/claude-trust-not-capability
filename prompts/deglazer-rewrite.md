# Rewrite the deglazer skill from scratch

## Context

You are working in the repo `claude-trust-not-capability`. It publishes Claude Code skills as prompt specifications. One skill, `/deglazer` at `skills/deglazer/SKILL.md`, removes signs of AI-generated writing from text.

The current version has IP problems. It derives content (specific examples, word lists, editorial framing) from two sources without adequate separation:

- **Wikipedia: Signs of AI writing** (CC BY-SA 4.0) — examples using real Wikipedia articles (Gallery 825, Statistical Institute of Catalonia, Korattur, a temple colour palette) and curated word lists
- **blader/humanizer** by Siqi Chen (MIT) — the v2.2.0 skill that the current version expanded from 24 to 37 patterns

Your job is to rewrite the skill from scratch so that every word list, every example, every diagnostic, and every piece of editorial framing is original. The underlying observations about AI writing patterns are factual and not copyrightable. The specific expression of those observations must be yours.

## What you can use

The academic papers contain factual research findings. You can read them and derive your own word lists, your own examples, and your own diagnostics from the same underlying observations. The facts ("AI text has more uniform sentence length than human text") are not owned by anyone.

Papers to consult:

- Linguistic Characteristics of AI-Generated Text (arXiv 2510.05136, 2025)
- The Disappearing Author: Linguistic and Cognitive Markers (ResearchLeap, 2025)
- Comparative linguistic analysis of human vs. machine-generated text (tandfonline, 2025)
- Liang et al., "Monitoring AI-Modified Content at Scale" (2025)
- Mitchell et al., DetectGPT follow-up work
- Gehrmann et al., GLTR lineage on token probability

You can also generate AI text yourself and study it directly. Prompting Claude, GPT, or any other model and examining the output for patterns is primary research, not derivation.

## What you must not use

- Any example text from the Wikipedia page (Gallery 825, Catalonia, Korattur, the temple, or any other real Wikipedia article used as an illustration on that page)
- Any word list copied from the Wikipedia page or from blader's skill
- Any before/after pair from either source
- Any editorial framing or diagnostic question lifted from either source

If a word list you produce happens to overlap partially with Wikipedia's (because the same words genuinely are overused by LLMs), that is fine as long as you compiled it independently from the research papers or your own analysis. Document how you derived each list.

## Constraints on the output skill

Read `CLAUDE.md` at the repo root before starting. Observe:

- British English throughout
- No emojis
- Tone: quiet authority — competent, clear, unpretentious
- The SKILL.md is both a readable specification and a Claude Code skill prompt. Everything in it must be something Claude needs during execution. Design rationale, research citations, and provenance go in `skills/deglazer/README.md`, not in the skill itself.

The skill frontmatter must include:

```yaml
---
name: deglazer
version: 5.0.0
description: |
  [your description]
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
---
```

## Goal

The rewritten skill must be at least as effective as the current version at making AI-generated text indistinguishable from skilled human writing. "At least as effective" is measured by the benchmark described below.

---

## The benchmark

Before writing any skill content, build a benchmark corpus. This corpus is the ground truth for every phase. No change ships unless it passes.

### Building the corpus

Create a file `tests/deglazer/corpus.md`. Generate 10 text samples by prompting an LLM (use yourself — ask Claude to write these as if it were producing final-draft content, not chatbot responses). Each sample should be 150-300 words.

Cover these domains and styles:

1. A technical blog post about a software architecture decision
2. A Wikipedia-style article about a small city or neighbourhood
3. A product announcement for a developer tool
4. A short essay arguing a position on AI in education
5. An internal company memo about a process change
6. A book review of a non-fiction title
7. A LinkedIn-style professional update
8. A documentation page explaining an API endpoint
9. A retrospective summary of a failed project
10. A persuasive essay about infrastructure investment

These 10 samples must remain unchanged throughout the rewrite. They are the fixed input.

### Scoring

After each phase, run the current state of the skill against all 10 samples. For each sample, record three scores (1-5 scale):

- **Detection resistance**: Would this pass as human-written to an attentive reader? (1 = obviously AI, 5 = indistinguishable)
- **Voice**: Does this sound like a specific person wrote it, or is it voiceless? (1 = sterile, 5 = distinctive)
- **Meaning preservation**: Is the original meaning intact? (1 = meaning distorted, 5 = fully preserved)

Record scores in `tests/deglazer/scores.md` as a table. Each phase adds a row per sample.

### Baseline

Run the existing skill (`skills/deglazer/SKILL.md` at its current state) against all 10 samples before making any changes. Record baseline scores. This is the bar to clear.

### Regression rule

After each phase, compare scores to the previous phase. The phase passes if:

- The mean detection resistance score across all 10 samples is equal to or higher than the previous phase
- No individual sample's detection resistance drops by more than 1 point
- The mean voice score does not drop
- The mean meaning preservation score does not drop

If a phase fails the regression test, revise it before moving on. Do not proceed with a regression.

---

## Phases

### Phase 0: Scaffold and benchmark

**Objective:** Create the benchmark corpus, run the baseline, and set up the skill skeleton.

Steps:

1. Read the current `skills/deglazer/SKILL.md` to understand what exists
2. Create `tests/deglazer/corpus.md` with 10 samples as specified above
3. Run the current skill against all 10 samples. Record baseline scores in `tests/deglazer/scores.md`
4. Create a new `skills/deglazer/SKILL.md` containing only the frontmatter, the title, the opening instruction ("You are a writing editor..."), and the overcorrect warning. No patterns yet
5. Run this skeleton skill against all 10 samples. Record scores. This is the "empty skill" baseline — it shows what Claude does with minimal instruction. The gap between this and the full baseline tells you how much the patterns actually contribute

**IP check:** The overcorrect warning must be originally worded. The concept (uniform application of humanisation is itself a tell) is not copyrightable. The specific bullet points and phrasing must be yours.

### Phase 1: Structural patterns

**Objective:** Add the patterns that address the shape and rhythm of AI text. These are the highest-signal patterns according to the research: the ones automated classifiers catch.

Patterns to cover (derive each independently from the papers and your own analysis):

- Sentence length uniformity
- Paragraph length uniformity
- Epistemic uniformity (uniform confidence level)
- Register consistency (no drift in formality)
- Punctuation distribution

For each pattern, write:

- A problem statement (what the pattern is and why it is a tell)
- A diagnostic (how to spot it)
- A before/after example using **original text you write yourself**, set in a software engineering or business context

Do not number the patterns yet. Numbering comes in the final phase when the full list is stable.

Steps:

1. Write the patterns
2. Add them to the skill under a "High detection risk" section
3. Run the skill against all 10 samples. Record scores
4. Compare to Phase 0 scores. Apply the regression rule
5. If any pattern made things worse (e.g., by encouraging overcorrection), revise it

**IP check:** For each pattern, note in `tests/deglazer/ip-log.md` where the observation came from (which paper, or "primary analysis of LLM output"). Confirm no examples or phrasing are from Wikipedia or blader.

### Phase 2: Vocabulary and surface patterns

**Objective:** Add patterns that address word choice and surface-level tells. This is where the current skill's IP exposure is highest (the word lists), so take particular care.

Patterns to cover:

- Overused AI vocabulary (compile your own list by prompting multiple LLMs and counting word frequencies, or by extracting from the academic papers' frequency analyses)
- Copula avoidance (the substitution of "serves as" / "stands as" for "is")
- Significance inflation (asserting arbitrary importance)
- Superficial participle phrases (-ing analyses)
- Causal over-assertion
- Vague attributions

For the vocabulary list: generate 20+ passages from different LLMs on different topics. Count which words appear disproportionately often. Build your list from that primary data. Document the method in `tests/deglazer/ip-log.md`.

Steps:

1. Conduct the vocabulary frequency analysis. Record method and raw data in `tests/deglazer/vocabulary-analysis.md`
2. Write each pattern with original examples
3. Add to the skill under a "Medium detection risk" section
4. Run against all 10 samples. Record scores
5. Apply regression rule

**IP check:** Compare your word list to the Wikipedia page's list. Overlap is expected (the same words genuinely are overused). But your list must be derived from your own analysis, not copied. The derivation log is your evidence.

### Phase 3: Polish patterns

**Objective:** Add lower-severity patterns that improve naturalness without being primary detection signals.

Patterns to cover:

- Promotional language
- Formulaic section headers
- Weak transitions and conjunctive phrases
- Bureaucratic bloat (wordy constructions)
- Intensifiers and adverb frontloading
- Formatting tells (em dashes, boldface, title case)
- Communication artifacts (chatbot residue)
- Hedging and sycophantic language

These are individually low-risk but collectively contribute to the "AI smell" of a text. Write them concisely — these do not need long problem statements or diagnostics. A word list and a one-line instruction for each is enough.

Steps:

1. Write each pattern
2. Add to the skill under a "Polish" section
3. Run against all 10 samples. Record scores
4. Apply regression rule. Watch for overcorrection — adding too many polish rules can make the skill overzealous

**IP check:** The bureaucratic bloat replacements ("in order to" becomes "to") are standard writing advice found in every style guide. They are not copyrightable. The formatting tells are factual observations. Flag any pattern where the phrasing feels too close to a specific source.

### Phase 4: Corpus-level patterns

**Objective:** Add patterns that only appear when multiple texts by the same "author" are compared. These are the hardest for models to avoid because they emerge from the generation process itself.

Patterns to cover:

- Cross-text verbatim repetition (near-identical sentences across pieces)
- Structural homogeneity (same rhetorical arc in every piece)
- Convergent framing devices (same construction at same structural position)
- Motif overuse (same concrete image recycled across pieces)

These require the skill to have a "collection mode" process distinct from single-text editing. Write the collection-mode process section.

Steps:

1. Write each pattern
2. Write the collection-mode process
3. Test: take the 10 benchmark samples and treat them as a collection. Run the corpus-level checks. Do you find cross-text issues? Record findings
4. Record scores. Apply regression rule

**IP check:** Cross-text patterns are original to the current skill (not in blader's 24 patterns). The concept is yours. Verify the framing and examples are original.

### Phase 5: Voice and personality diagnostics

**Objective:** Add the diagnostics that address voiceless writing. These are not pattern-removal rules — they are generative instructions that add humanity.

Diagnostics to cover:

- Emotional flatness (no reaction to the material)
- Opinion absence (neutral to the point of vacuity)
- Absence of specific detail (generic examples instead of concrete ones)
- Temporal flattening (text that floats outside time)

For each, write a diagnostic question and a before/after example. The "after" examples are the hardest part to get right — they must sound genuinely human, not performatively human.

Steps:

1. Write each diagnostic
2. Add to the skill under a "Voice and personality" section
3. Run against all 10 samples. Record scores. The voice score should improve noticeably in this phase
4. Apply regression rule

**IP check:** These diagnostics were original additions in the current skill (not from blader or Wikipedia). Verify that the examples are new.

### Phase 6: Process, integration, and final testing

**Objective:** Write the process section, assign final numbering, and run the complete benchmark.

Steps:

1. Write the single-text process (read first, tiers in order, deliberate imperfections)
2. Write the collection process (corpus-level checks before individual editing)
3. Write the output format section
4. Number all patterns sequentially
5. Read the complete skill end to end. Check for: internal consistency, tone, conciseness, British English, no emojis
6. Run the full skill against all 10 samples. Record final scores
7. Compare to baseline (Phase 0, current skill). The final scores must meet or beat the baseline on all three dimensions
8. If any dimension is lower, identify which patterns are underperforming and revise

**IP check — final audit:**

1. Read the entire skill and confirm that no example text, word list, or diagnostic question is recognisably from the Wikipedia page or blader's skill
2. For each word list, confirm the derivation is logged in `tests/deglazer/ip-log.md`
3. Check that no before/after pair uses a real Wikipedia article as its subject matter
4. Write a summary in `tests/deglazer/ip-log.md` stating: sources consulted, method of derivation for each original element, confirmation that no CC BY-SA content was carried over

### Phase 7: Documentation

**Objective:** Update the README and NOTICE file.

1. Update `skills/deglazer/README.md` to reflect the new version. The provenance section should state that this is an independent rewrite, note the academic papers consulted, and describe the derivation method for word lists
2. Update the `NOTICE` file at the repo root. The blader/humanizer attribution should be replaced with a note that v5.0.0 is an independent rewrite that does not derive from that work. The Wikipedia attribution can be removed if no CC BY-SA content remains
3. Update the version in the SKILL.md frontmatter to 5.0.0

---

## Working rules

- **Do not skip the benchmark.** Every phase ends with a test run. No exceptions.
- **Do not batch phases.** Complete one, test it, pass the regression check, then start the next.
- **Do not look at the current skill while writing the new one.** Read it once in Phase 0 to understand what exists, then work from the academic papers and your own analysis. If you catch yourself recalling a specific example or phrasing from the current skill, write something different.
- **Commit after each phase.** Each commit should be a self-contained improvement that passes the benchmark.
- **When in doubt, cut.** A skill with 25 well-tested patterns is better than one with 37 that includes filler. Not every pattern from the current version needs to survive. If a pattern does not measurably improve detection resistance, drop it.
- **Original examples are the hard part.** Invest time here. Write examples set in contexts you know: software engineering, team dynamics, product decisions, infrastructure. Do not use examples about cities, temples, galleries, or other Wikipedia-article subjects.

## Research

Use web search to read the academic papers listed in the "What you can use" section. Do not rely solely on training data for the research findings. Fetch the actual papers and extract specific data points, frequency counts, and pattern descriptions. Your word lists and diagnostics should cite specific findings from specific papers, not vague recollections.

## Session discipline

- **Read `CLAUDE.md` at the repo root before doing anything else.** It defines the writing conventions, tone, and structure for this repo. Everything you produce must conform to it.
- **Do not read `skills/deglazer/SKILL.md` after Phase 0.** You read it once to understand the scope and to run the baseline. After that, close it. Work from the papers and your own analysis only. If you find yourself reproducing a specific phrase or example from the current skill, stop and write something different.
- **If a phase fails the regression rule, fix it before moving on.** State what failed, what you think caused the regression, and what you changed. Do not silently adjust scores or skip the check.
- **Commit and push after each phase.** Each commit message should name the phase and summarise what was added or changed.
