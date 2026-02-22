# Stet tests

Manual tests for the `/stet` skill. Run these when changing the skill specification or to verify behavior on a new model.

## Test corpus

**Single text** (`single.md`): A blog post about microservices migration, seeded with patterns from all severity tiers. Tests patterns 1-16 and 24-27.

**Collection** (`collection-a.md`, `collection-b.md`, `collection-c.md`): Three engineering blog posts on related topics. They share verbatim phrases, identical structural arcs, the same closing rhetorical device, and a recurring house/foundation metaphor. Tests corpus-level patterns 20-23 in addition to per-piece patterns.

## Running the single-text test

Set up a test repo with the skill installed:

```
mkdir -p test-repo/.claude/skills/stet
cp ../SKILL.md test-repo/.claude/skills/stet/SKILL.md
cp single.md test-repo/article.md
cd test-repo
```

Invoke the skill:

```
/stet article.md
```

Check the output against `rubric.md`, "Single text" section.

## Running the collection test

Same setup, but copy all three collection files:

```
cp collection-a.md collection-b.md collection-c.md test-repo/
```

Invoke the skill on the collection:

```
/stet collection-a.md collection-b.md collection-c.md
```

Check against `rubric.md`, "Collection" section. The skill should report cross-text issues before editing individual pieces.

## What passing looks like

The skill is non-deterministic. Each run produces different edits. A passing result:

- Addresses all high-detection-risk patterns (1-5)
- Catches most medium-risk and polish patterns (6-16)
- Identifies cross-text repetition and structural homogeneity in the collection
- Does not overcorrect (some minor patterns deliberately left untouched)
- Produces text with genuine voice: opinions, specific details, varied emotional register
- Preserves the factual content and intent of each piece

The rubric includes a prompt for evaluating output with a model if you prefer not to check manually.

## Results

Dated evaluation runs with scores, commentary, and comparison charts are in [`results.md`](results.md). Each run records the method (full invocation vs. paraphrased), model, per-pattern verdicts, and aggregate stats. Add new runs as you iterate on the spec or test against new models.
