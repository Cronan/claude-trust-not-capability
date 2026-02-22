# Test results

Dated evaluation runs of the `/stet` skill against the test corpus. Each run records method, model, scores, and commentary. The reference outputs in this directory were generated during run 001.

Comparison across runs tracks whether spec changes improve output quality or introduce regressions.

-----

## Run 001 -- 2026-02-22

**Method:** Paraphrased spec (not full `/stet` invocation). The SKILL.md was summarized into an agent prompt rather than loaded as the complete skill. The agents received the gist of the 27 patterns but not the full 451-line specification with all diagnostics, before/after examples, and exact word lists.

**Model:** Claude Opus 4 (claude-opus-4-20250514)

**Invocation:** Three parallel subagents (single text, collection corpus analysis, collection individual edits). Each received a condensed version of the spec plus the input files.

### Single text scores

| # | Pattern | Verdict | Notes |
|---|---|---|---|
| 1 | Sentence length uniformity | PASS | 9 sentences under 8 words including "The pitch is fine." (4w) and "How it actually does." (4w). One sentence at 37 words ("The problem is that the pitch skips over the parts that will hurt..."). Good range: 4-37 words. |
| 2 | Paragraph length uniformity | FAIL | 12 paragraphs, all between 2-5 sentences. No single-sentence paragraph, no 6+ sentence paragraph. The band shifted from 3-4 to 2-5 but never broke out of it. |
| 3 | Epistemic uniformity | PASS | "hard to say," "probably," "roughly once a sprint," "I'm not sure" (via hedged phrasing). Multiple uncertainty markers. |
| 4 | Register consistency | PASS | "the parts that will hurt," "sliced along the wrong seams," "usually on a Friday afternoon." Clear informal shifts. |
| 5 | Punctuation distribution | PASS | 2 semicolons, 3 colons, 4 parenthetical pairs. Missing: question marks. Three of four punctuation types present. |
| 6 | Overused AI vocabulary | OVERCORRECTED | 0 of 16 flagged words survive. 100% removal. Rubric asks 80%. The spec says "leave some instances untouched" -- the model ignored this. |
| 7 | Copula avoidance | PASS | 0 of 5 copula avoidance patterns survive. All replaced with direct verbs. |
| 8 | Significance inflation | PASS | 0 of 5 inflation phrases survive. |
| 9 | Superficial participle phrases | PASS | Both removed. |
| 10 | Causal over-assertion | PASS | "As a result" and "which led to" both replaced with weaker connections. |
| 11 | Vague attributions | PASS | "Industry experts suggest" replaced with "I've seen numbers from Thoughtworks and the DORA reports." |
| 12 | Promotional language | PASS | All three ("transformative potential," "operational excellence," "innovative solutions") removed. |
| 13 | Formulaic section headers | PASS | "Why This Matters" and "Looking Ahead" replaced with "The Real Reason Teams Struggle," "Getting Service Boundaries Right (or Close Enough)," "What Comes After the Migration." |
| 14 | Weak transitions | PASS | 0 of 4 survive. Furthermore, Moreover, Additionally, Indeed all gone. |
| 15 | Bureaucratic bloat | PASS | All four replaced with plain equivalents. |
| 16 | Intensifiers | PASS | Both "significantly" and "Importantly" removed. |
| 24 | Emotional flatness | PASS | Frustration about saga debugging, skepticism about conference talks, humor in "usually on a Friday afternoon." |
| 25 | Opinion absence | PASS | "If your teams can't ship a monolith reliably, distributing the problem across thirty services will make things worse, not better." Clear stance. |
| 26 | Absence of specific detail | PASS | Thoughtworks, DORA reports, Pact, Kafka, RabbitMQ, "roughly once a sprint," "six months." |
| 27 | Temporal flattening | PASS | "Last year I watched a team spend three months," "six months later," "within about two quarters." |
| OC | Overcorrection (vocabulary) | FAIL | Zero flagged words is itself a tell. No human editor removes every instance of "robust" or "innovative" -- some uses are legitimate. |
| OC | Overcorrection (structure) | FAIL | No paragraph breaks the 2-5 sentence band. 12 paragraphs all land in a safe range. |
| OC | Overcorrection (long sentences) | PASS | Max 37 words. Just clears the 35-word threshold, though only one sentence breaks it. Human writing typically has more. |

**Single text summary: 19 PASS, 3 FAIL (vocabulary overcorrection, structural overcorrection, paragraph uniformity)**

### Collection scores

| # | Pattern | Verdict | Notes |
|---|---|---|---|
| 20 | Verbatim repetition | PASS | Zero 8+ word phrases shared across any pair. Complete phrasal differentiation. |
| 21 | Structural homogeneity | PASS | Three different openings (anecdote / historical / observation). Three different closings (patience reflection / "none of this is fast" / reliability lesson). |
| 22 | Convergent framing devices | PASS | "The question isn't whether X" construction appears in zero pieces. |
| 23 | Motif overuse | PASS | House metaphor appears in zero pieces. Trust framing varies: piece A uses it directly, piece B frames it as respect between reviewer/author, piece C frames it as developer confidence in tools. |
| SS | Paragraph count uniformity | FAIL | All three pieces have exactly 5 paragraphs. The input had 5 paragraphs each. The structural silhouette is unchanged. |
| SS | Sentence count uniformity | PASS | 18, 20, 24 -- sufficient spread. |
| FC | Fabrication check | FLAG | "One director I knew blocked her Thursday afternoons" (piece A), "one senior engineer's habit of leaving sarcastic review comments" (piece B), "a company with 200 engineers running eleven different CI configurations" (piece C). All plausible, all invented. A real user would need to replace these with actual experiences. |

**Per-piece quality (patterns 1-19, 24-27):**

| Metric | Piece A | Piece B | Piece C |
|---|---|---|---|
| Word count | 304 | 328 | 313 |
| Paragraphs | 5 | 5 | 5 |
| Sentences | 18 | 20 | 24 |
| Sentence range (words) | 6-36 | 4-37 | 1-35 |
| Sentences under 8 words | 2 | 5 | 10 |
| Sentences over 35 words | 1 | 2 | 0 |
| AI vocab surviving | 0 | 0 | 0 |
| AI phrases surviving | 0 | 0 | 0 |

**Collection summary: 5 PASS, 1 FAIL (paragraph uniformity), 1 FLAG (fabrication)**

### Aggregate scorecard

| Category | Items | Pass | Partial | Fail | Flag |
|---|---|---|---|---|---|
| High detection risk (1-5) | 5 | 4 | 0 | 1 | 0 |
| Medium detection risk (6-11) | 6 | 5 | 0 | 1 | 0 |
| Polish (12-16) | 5 | 5 | 0 | 0 | 0 |
| Voice (24-27) | 4 | 4 | 0 | 0 | 0 |
| Overcorrection | 3 | 1 | 0 | 2 | 0 |
| Cross-text (20-23) | 4 | 4 | 0 | 0 | 0 |
| Structural silhouette | 2 | 1 | 0 | 1 | 0 |
| Fabrication | 1 | 0 | 0 | 0 | 1 |
| **Total** | **30** | **24** | **0** | **5** | **1** |

### Commentary

**What worked well.** The skill's core competence -- identifying and replacing AI vocabulary, dismantling structural cliches, injecting voice -- is strong even with a paraphrased spec. Cross-text deduplication was thorough: zero shared phrases, completely different narrative arcs, varied metaphors. Voice injection was the standout: specific tool references, temporal anchors, genuine opinions, and emotional register all landed.

**What didn't work.** Overcorrection is the dominant failure mode. The model treats "remove AI patterns" as "remove all AI patterns" despite the spec explicitly saying otherwise. This manifests in three ways:

1. *Vocabulary stripping is total.* The spec says 80% removal and "leave some instances untouched." The model achieved 100%. Zero of 16 flagged words survived in the single text. Zero AI phrases in any output. A human editor would leave a "robust" or "comprehensive" that fits naturally -- the model can't resist removing them all. A perfect sweep is itself a fingerprint.

2. *Paragraph structure stays safe.* Every paragraph in the single text lands in the 2-5 sentence range. All three collection pieces are exactly 5 paragraphs. The model diversified sentence lengths at the short end (9 sentences under 8 words in the single text) and produced one 37-word sentence, but the overall range remains narrower than typical human writing. Real writing has occasional 50-word sentences that work. The structural silhouette of the collection is identical to the input.

3. *The "do not overcorrect" instruction is aspirational.* The spec devotes a full section to this and the model treats it as secondary to the pattern-removal instructions. This is likely inherent to how models process competing objectives -- the specific, measurable instruction (remove these words) wins over the general, subjective one (but not too thoroughly).

**What we can improve.**

*In the spec:* Strengthen the overcorrection instructions. Consider specifying a maximum removal percentage (e.g., "remove no more than 90% of flagged vocabulary") rather than a minimum. Add a structural diversity requirement: "at least one paragraph should exceed 6 sentences; at least one sentence should exceed 40 words." Frame the collection paragraph count as an explicit check: "the output pieces must not all share the same paragraph count."

*In the rubric:* Already done in this run -- added paragraph-count uniformity, vocabulary overcorrection, long sentence check, and fabrication check.

*In the test:* A full invocation using the actual `/stet` skill (loading the complete SKILL.md) would be a fairer test. The paraphrased approach was useful as a baseline but undersells the spec.

**Fabricated specifics.** The model invented plausible details (a director's Thursday afternoons, 200 engineers with eleven CI configs, a sarcastic reviewer) because the spec asks for specific detail but the input provides none. This is not a bug -- it is the correct behavior when editing text with no real specifics to surface. The writer should replace these with their actual experiences. Worth noting in the skill README.

-----

## Comparison charts

### Pattern pass rates by category

```
                        Run 001
                        -------
High detection risk     ||||||||||...  80%  (4/5)
Medium detection risk   ||||||||||...  83%  (5/6)
Polish                  |||||||||||||  100% (5/5)
Voice and personality   |||||||||||||  100% (4/4)
Overcorrection          ||||.........  33%  (1/3)
Cross-text patterns     |||||||||||||  100% (4/4)
Structural silhouette   ||||||.......  50%  (1/2)

Overall                 ||||||||||...  80%  (24/30)
```

### Overcorrection detail

```
                        Run 001
                        -------
Vocabulary removal %    |||||||||||||  100%  target: 80-95%
Max sentence length     ||||||||||..   37w   target: 35w+  (borderline)
Paragraph band breaks   .............  0     target: 1+
Collection para counts  5 / 5 / 5           target: varied
```

### Per-piece sentence length distribution (collection)

```
Words   Piece A          Piece B          Piece C
0-7     ##               #####            ##########
8-14    ####             #####            ########
15-21   ######           #####            ####
22-28   ####             ###              #
29-35   #                ##               #
36+     #                ##

Mean    16.9             16.4             13.0
```

Each `#` represents one sentence. Piece C is the most varied (1-word sentence "Eleven." pulls the min down). Piece A and B cluster in the middle. None achieve the long tail that human writing typically shows.

### Tracking template

Future runs add columns. The comparison shows whether spec or model changes improve the overcorrection problem.

```
Metric                          Run 001    Run 002    Run 003
------------------------------ ---------- ---------- ----------
Method                          paraphrase
Model                           opus-4
Pass rate (overall)             80%
Pass rate (pattern removal)     90%
Pass rate (overcorrection)      33%
Vocab removal %                 100%
Max sentence (single)           37w
Paragraph band breaks           0
Collection para uniformity      5/5/5
Fabricated details              3
Cross-text shared phrases       0
```

-----

## How to add a new run

1. Run the test (full invocation or otherwise). Note the method and model.
2. Evaluate against the rubric. Use the evaluation prompt in `rubric.md` or score manually.
3. Add a new `## Run NNN` section above the comparison charts.
4. Update the charts with the new column.
5. If the run reveals a new weakness, add a rubric check and note it in the commentary.
