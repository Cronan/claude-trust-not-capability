# Deglazer

A writing editor skill that identifies and corrects signs of AI-generated text. Covers 37 patterns organised into three severity tiers by detection risk, plus voice and personality diagnostics.

## Provenance

This skill builds on [humanizer](https://github.com/blader/humanizer) by Siqi Chen (MIT licence). That skill identified 24 patterns drawn from the Wikipedia guide and academic research listed below.

This version reorganises those patterns into severity tiers (high, medium, polish), adds 13 patterns (corpus-level detection, epistemic uniformity, register consistency, lexical diversity, punctuation poverty, the clean opening problem, absence of self-correction, and four voice diagnostics), and adds the "don't overcorrect" constraint to prevent uniform application from becoming its own tell.

## References

- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by WikiProject AI Cleanup
- [Linguistic Characteristics of AI-Generated Text: A Survey (arXiv, 2025)](https://arxiv.org/pdf/2510.05136)
- [The Disappearing Author: Linguistic and Cognitive Markers (2025)](https://researchleap.com/the-disappearing-author-linguistic-and-cognitive-markers-of-ai-generated-communication/)
- [Comparative linguistic analysis of human vs. machine-generated text (2025)](https://www.tandfonline.com/doi/full/10.1080/09540091.2025.2507183)
- Liang et al., "Monitoring AI-Modified Content at Scale" (2025) — distributional and causal language patterns
- Mitchell et al., DetectGPT follow-up work on perplexity-based detection
- Gehrmann et al., GLTR lineage on uniform token probability

As reasoning models improve, surface vocabulary tells fade, but structural patterns (uniform sentence length, epistemic flatness, register lock) and corpus-level patterns (cross-text repetition, structural homogeneity) persist. The strongest tells are now about texture, not vocabulary.

## Licence note

The Wikipedia page is CC BY-SA 4.0. Several examples and word lists in the skill are derived from that page. See the NOTICE file at the repository root for licence details.
