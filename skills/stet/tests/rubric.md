# Evaluation rubric

Criteria for evaluating stet output against the test corpus. Each item specifies what the input contains and what a passing edit looks like.

Output is non-deterministic. Not every run will produce identical edits. A run passes if it meets the majority of criteria below and does not overcorrect.

-----

## Single text: single.md

### High detection risk

**1. Sentence length uniformity**
- Input: nearly all sentences fall in the 15-25 word band
- Pass: output contains at least two sentences under 8 words and at least one over 35 words

**2. Paragraph length uniformity**
- Input: all paragraphs are 3-4 sentences
- Pass: at least one single-sentence paragraph or a paragraph of 6+ sentences

**3. Epistemic uniformity**
- Input: every claim stated with equal certainty
- Pass: at least two instances of hedging, doubt, or approximation ("probably," "hard to say," "around," "I'm not sure")

**4. Register consistency**
- Input: sustained formal/academic register throughout
- Pass: at least one informal phrase, aside, or shift in tone

**5. Punctuation distribution**
- Input: almost exclusively commas, periods, and occasional dashes
- Pass: at least two of: semicolons, colons, parentheses, question marks

### Medium detection risk

**6. Overused AI vocabulary**
- Input contains: comprehensive (x2), robust, leverage, foster, meticulous, crucial (x3), pivotal, showcase, delve, innovative (x2), noteworthy, navigate (x2), landscape, paradigm, seamless, resilient
- Pass: at least 80% replaced or removed

**7. Copula avoidance**
- Input contains: "serves as," "functions as," "stands as," "feature ... that offers," "plays a vital role"
- Pass: at least 3 of 5 replaced with "is," "has," or plain verb

**8. Significance inflation**
- Input contains: "a testament to," "pivotal moment," "underscores the growing importance," "plays a vital role," "setting the stage for"
- Pass: at least 4 of 5 removed or deflated

**9. Superficial participle phrases**
- Input contains: "highlighting the importance of adaptability," "fostering a culture of ownership"
- Pass: both removed or rewritten as independent clauses

**10. Causal over-assertion**
- Input contains: "As a result," "which led to"
- Pass: at least one replaced with weaker or more honest connection

**11. Vague attributions**
- Input contains: "Industry experts suggest"
- Pass: replaced with a specific source or owned as the author's claim

### Polish

**12. Promotional language**
- Input contains: "transformative potential," "operational excellence," "innovative solutions"
- Pass: all three removed or replaced with concrete descriptions

**13. Formulaic section headers**
- Input contains: "Why This Matters," "Looking Ahead"
- Pass: both replaced with content-specific headers or removed

**14. Weak transitions**
- Input contains: Furthermore, Moreover, Additionally, Indeed
- Pass: at least 3 of 4 removed or replaced with "and," "but," "so," or implicit connection

**15. Bureaucratic bloat**
- Input contains: "Due to the fact that," "in order to," "is able to," "has the potential to"
- Pass: all four replaced with plain equivalents (because, to, can, could)

**16. Intensifiers**
- Input contains: "significantly," "Importantly"
- Pass: both removed or the sentences rewritten to carry meaning without them

### Voice and personality

**24. Emotional flatness**
- Input: no emotional reactions anywhere
- Pass: at least one moment of surprise, frustration, humor, or skepticism

**25. Opinion absence**
- Input: balanced presentation with no stance
- Pass: at least one clear opinion or recommendation

**26. Absence of specific detail**
- Input: generic "organizations," "teams," no specifics
- Pass: at least one specific detail (a tool name, a number, a concrete scenario)

**27. Temporal flattening**
- Input: vague present tense throughout
- Pass: at least one temporal anchor ("last year," "in Q3," "when we started")

### Overcorrection check

- At least one minor AI pattern should remain (a single transitional adverb, one instance of copula avoidance, etc.)
- The text should not read as though every sentence was individually optimized
- Factual content and intent of the original should be preserved

**Vocabulary overcorrection**
- Fail if 100% of flagged vocabulary is removed. The spec says "leave some instances untouched." A perfect sweep is itself a fingerprint -- it signals automated processing rather than human editing. Passing range: 80-95% removal.

**Structural overcorrection**
- Paragraph count should differ from input by at least 2 (input has 7 content paragraphs). If the output still has exactly the same paragraph count, the structural rewrite was cosmetic.
- At least one paragraph should break the 2-5 sentence band. If every paragraph lands in the same range, the uniformity has simply shifted rather than resolved.

**Long sentence check**
- Output should contain at least one sentence over 35 words. Real human writing has occasional run-on or complex sentences. Capping everything at 30 words is its own form of uniformity.

-----

## Collection: collection-a.md, collection-b.md, collection-c.md

### Cross-text patterns

**20. Verbatim repetition**
- Input shares across all three pieces:
  - "The landscape of X has evolved significantly"
  - "share several key characteristics"
  - "The role of X cannot be overstated"
  - "Organizations are increasingly recognizing"
  - "Furthermore, teams that"
  - "Comprehensive X further reinforce"
- Pass: no phrase of 8+ words appears verbatim in more than one output piece

**21. Structural homogeneity**
- Input: all three follow identical arc (context-setting -> characteristics -> leadership role -> house metaphor -> rhetorical question conclusion)
- Pass: at least one piece has a different opening move and at least one has a different closing move

**22. Convergent framing devices**
- Input: all three end with "The question isn't whether X. It's whether Y"
- Pass: at most one piece retains this construction

**23. Motif overuse**
- Input: "building a house" / "foundation" metaphor in all three; "trust" as recurring theme in all three
- Pass: the house metaphor appears in at most one piece; "trust" framing varies across pieces

### Structural silhouette

**Paragraph count uniformity**
- Fail if all three output pieces have exactly the same paragraph count. The input pieces all have 5 paragraphs. If the outputs all have 5 paragraphs (or all have 4, or all have 6), the structural homogeneity has survived the edit. At least one piece should have a different paragraph count from the others.

**Sentence count uniformity**
- Fail if the sentence counts across the three pieces fall within a 2-sentence range (e.g. 18, 19, 20). Some variation is expected -- the pieces cover different topics at different depths.

### Per-piece quality

Each collection piece should also pass the single-text criteria (patterns 1-19, 24-27) individually.

### Fabrication check

- Specific details added by the skill (names, numbers, anecdotes) should be plausible but are necessarily invented. Note any detail that reads as a factual claim but has no basis in the input. This is not a fail condition -- it is a limitation to flag. The writer should replace fabricated specifics with real ones.

-----

## Running the evaluation

After editing, check the rubric manually or use this evaluation prompt with any capable model:

```
I have an original text, an edited version, and an evaluation rubric.

Original:
[paste original]

Edited:
[paste edited output]

Rubric:
[paste relevant rubric section]

For each rubric item, report PASS or FAIL with a brief explanation.
At the end, note any overcorrection: places where the edit is too uniform,
too clean, or has lost the original's factual content.
```
