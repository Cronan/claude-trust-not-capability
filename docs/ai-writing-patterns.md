# The 27 ways AI overwrites your voice

There is a word for what happens when an LLM hallucinates: it sees what is not there. A recent Register piece coined a word for the opposite problem: *semantic ablation* -- the model destroying what is there. Your specific phrasing, your uneven rhythm, your willingness to leave something unresolved. The model smooths all of it into the same three-paragraph shape with the same measured confidence about everything.

The result is not bad writing in the traditional sense. It is competent, grammatical, and organized. It is also indistinguishable from every other piece of AI-assisted text published this week. That is the problem. Not detection. Quality.

## The wrong framing

The market has split AI writing tools into two camps: detectors (Turnitin, GPTZero, Copyleaks) and humanizers (BypassGPT, Phrasly, and dozens of others that rewrite text to evade those detectors). This is an arms race framed around the signal, not the substance. A humanizer that swaps "delve" for "explore" and varies sentence length to fool a classifier has not fixed the writing. It has laundered it.

The actual problem is simpler and harder to game: AI-assisted text is often worse than what you would have written yourself. Not because AI cannot write -- it can, fluently -- but because its fluency is generic. It writes the median response to your prompt. The most statistically likely phrasing. The most commonly rewarded structure. Your writing, whatever its faults, was at least yours.

## What goes wrong, specifically

After reviewing published research on linguistic markers of AI text, generating test corpora, and cross-referencing against human writing of equivalent genre and length, we identified 27 recurring patterns. They are not equally damaging. Some make entire passages feel monotonous; others are individually minor but accumulate.

### Structural patterns

The patterns that do the most damage are not about vocabulary. They are about shape.

**Sentence length uniformity.** Human prose has high variance in sentence length. A short sentence lands hard. A longer one meanders, qualifies, doubles back. AI text clusters sentences around a narrow band -- typically 15 to 25 words each -- producing a metronomic rhythm. Research on distributional properties of AI text consistently finds that sentence length standard deviation is one of the strongest classifiers, but it is also one of the strongest quality indicators: monotonous rhythm is monotonous to read.

**Paragraph length uniformity.** Squint at a page of AI text and every paragraph is the same height. Three to five sentences, tiling neatly like bricks. Human writing is visually ragged: a one-sentence paragraph for emphasis, a long block when the argument requires it, a two-line aside.

**Epistemic uniformity.** Every claim carries identical confidence. The author is equally certain about everything. Human writers hedge about some things, commit fully to others, and occasionally admit they do not know. Flat certainty across every point reads as synthetic because it is.

**Register consistency.** Human formality drifts. A technical writer drops into slang when recounting something frustrating. A casual writer tightens up when the stakes are high. AI maintains one register from start to finish.

**Punctuation distribution.** Commas, periods, and dashes account for nearly all AI punctuation. Human text uses semicolons, colons, parentheses, question marks mid-paragraph. Low punctuation entropy is a measurable signal, but it is also a reading-experience problem: varied punctuation controls pacing.

### Vocabulary and construction

**Overused vocabulary.** Words like "delve," "comprehensive," "leverage," "foster," "nuanced," and "seamless" appear at frequencies that are 10-35x higher in post-2023 generated text than in equivalent human writing. Corpus studies tracking millions of academic papers documented fold-increases of 25x for "delves" between 2022 and 2024. Any two of these in the same paragraph is a strong signal. Three is almost diagnostic.

**Copula avoidance.** "Serves as," "functions as," "stands as" where "is" would do. The model avoids the most common verb in English as though it were inelegant. It is not.

**Significance inflation.** Everything is pivotal, crucial, transformative. If everything is significant, nothing is. Human writers reserve emphasis for things that warrant it.

**Causal over-assertion.** Clean because/therefore chains where reality is messy. Humans say "and then" or "around the same time" when the causal relationship is unclear. AI text narrates causation as though it were obvious.

**Vague attributions.** "Industry experts suggest," "observers have noted," "research indicates" without citation. Human writers either name their source or own the opinion.

### Surface patterns

Individually low-risk, collectively diagnostic: promotional language ("groundbreaking," "revolutionary"), formulaic headers ("Why this matters," "Looking ahead"), weak transitions ("Furthermore," "Moreover"), bureaucratic bloat ("in order to," "due to the fact that"), adverb frontloading ("Interestingly," "Notably"), em dash overuse, and chatbot residue ("I hope this helps").

### What is missing

Stripping patterns is half the work. The other half is absence.

**Emotional flatness.** Does the writer react to their own material? A human writing about a failed migration is annoyed or reflective. AI text reports without reacting.

**Opinion absence.** Every side presented with equal weight, conclusion left to the reader. Human writers lean somewhere. Even cautious ones.

**Absence of specific detail.** "A team I worked with" versus "the four of us at the tail end of that November sprint." Generic examples could come from any prompt. Specific details are too particular to fabricate efficiently.

**Temporal flattening.** Everything in vague present tense. "Organizations are increasingly adopting" avoids committing to any moment. Human writing is anchored in time.

### Corpus-level patterns

When you compare multiple texts by the same AI-assisted "author," a new category emerges. Each text was generated independently from a similar distribution, so they converge in ways a single human's work would not: verbatim phrases repeated across pieces, identical rhetorical arcs, the same framing devices at the same structural positions, recurring metaphors.

These are the hardest patterns for a model to avoid because they emerge from the generation process itself. They are also invisible when editing a single text -- you only see them across a collection.

## The editorial approach

The alternative to both detection and evasion is editing. Not "make this text pass a classifier" but "make this text better."

The distinction is practical, not just ethical. A humanizer that varies sentence length to beat GPTZero has not fixed the underlying monotony -- it has added noise. An editorial pass that varies sentence length because the writing reads better with variation has fixed the actual problem. The text passes classifiers as a side effect, but the goal was never to pass classifiers. The goal was prose that sounds like a specific person wrote it.

This is what [stet](../skills/stet/) does. It works through the 27 patterns in severity order, strips the most damaging ones, and adds what is missing: opinion, emotional reaction, specific detail, temporal anchoring. Critically, it does not apply every rule to every text. A piece with zero rough edges is itself suspicious. Imperfection is a feature.

## The overcorrection problem

An LLM editing LLM output has a specific failure mode: it applies corrections uniformly, creating a new detectable pattern. Replacing every "Furthermore" with "And" is not editing. It is search-and-replace. Removing 100% of a vocabulary list instead of 80% reads as scrubbed rather than written.

In testing, overcorrection was the dominant failure mode. The spec now includes explicit constraints: leave some instances of minor patterns untouched (a human editor misses things), vary priorities from one text to the next, respect the writer's existing habits. The goal is a text that sounds like a particular person, not like an idealized average of all human writing.

## What this is not

This is not a detection evasion tool. If you are submitting AI-generated work as your own in a context where that is dishonest, stet does not change the ethics of that. It is also not a replacement for writing. If you did not have something to say before you opened the chat window, no amount of editing will fix that.

It is a tool for people who used AI to help them draft something -- because the first draft is tedious, because they think better by reacting to text than by staring at a blank page, because they are writing in a second language -- and want the result to actually sound like them. That is a legitimate editorial workflow. It has been legitimate since the first writer hired a typist.

## References

The pattern taxonomy draws on:

- Liang et al., [Monitoring AI-Modified Content at Scale](https://arxiv.org/abs/2403.07183) (ICML 2024) -- vocabulary frequency shifts
- [Linguistic Characteristics of AI-Generated Text: A Survey](https://arxiv.org/pdf/2510.05136) (arXiv 2025) -- sentence length distributions, punctuation entropy
- [The Disappearing Author: Linguistic and Cognitive Markers](https://researchleap.com/the-disappearing-author-linguistic-and-cognitive-markers-of-ai-generated-communication/) (2025) -- epistemic and affective markers
- Mitchell et al., [DetectGPT](https://arxiv.org/abs/2301.11305) (ICML 2023) -- perplexity distributions
- Gehrmann et al., [GLTR](https://aclanthology.org/P19-3019/) (ACL 2019) -- token probability uniformity
- The Register, [Semantic Ablation](https://www.theregister.com/2026/02/16/semantic_ablation_ai_writing/) (Feb 2026) -- the term for AI destroying specificity

The word lists were compiled by generating 20 passages across varied topics and registers, counting which words appeared disproportionately often relative to equivalent human writing, and cross-referencing against the published corpus studies.
