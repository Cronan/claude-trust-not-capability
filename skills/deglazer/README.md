# Deglazer

A writing editor skill that identifies and corrects signs of AI-generated text. Covers 27 patterns organized into three severity tiers by detection risk, plus corpus-level checks and voice diagnostics.

## Research references

The following academic papers informed the pattern descriptions and diagnostics:

- [Linguistic Characteristics of AI-Generated Text: A Survey (arXiv, 2025)](https://arxiv.org/pdf/2510.05136) — sentence length distributions, punctuation entropy, vocabulary frequency
- [The Disappearing Author: Linguistic and Cognitive Markers (2025)](https://researchleap.com/the-disappearing-author-linguistic-and-cognitive-markers-of-ai-generated-communication/) — epistemic uniformity, affective markers, cognitive markers
- [Comparative linguistic analysis of human vs. machine-generated text (2025)](https://www.tandfonline.com/doi/full/10.1080/09540091.2025.2507183) — structural regularity, stylistic consistency
- Liang et al., "Monitoring AI-Modified Content at Scale" (2025) — causal language distributions
- Mitchell et al., DetectGPT follow-up work — perplexity-based detection
- Gehrmann et al., GLTR lineage — token probability uniformity

## Word list derivation

The overused vocabulary list (pattern 6) and other word lists were compiled by generating 20 passages across varied topics and registers, then counting which words appeared disproportionately often relative to equivalent human writing, and cross-referencing against the published corpus studies listed above.
