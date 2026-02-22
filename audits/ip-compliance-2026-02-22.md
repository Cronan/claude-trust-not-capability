# IP Compliance Audit — 2026-02-22

Audit of all skills in this repository for intellectual property risk, conducted before offering skills for use in a workplace setting.

## `/deep-init` — LOW RISK

The methodology (two-phase git reconnaissance, parallel subagent extraction, cross-referencing by ticket ID, the rule test, the synthesis process) is original prompt engineering. No other published skill or tool uses this approach.

References are standard and properly handled:

- IFScale (July 2025) — cited as a factual research finding about instruction-following limits
- HumanLayer — cited as a point of comparison for CLAUDE.md line counts
- Brooks, Conway's Law, Deming's PDSA, DORA/Accelerate — standard industry references in the README, not in the skill itself
- `anthropics/skills` — mentioned as a directory convention to follow, not content

The illustrative examples (stock reservations, warehouse API, discount application, pre-orders) are fictional e-commerce scenarios, not taken from any existing repository.

No IP concerns.

## `/deglazer` — SIGNIFICANT RISK

Two separate problems identified.

### Problem 1: Unattributed derivation from blader/humanizer

A repository called [blader/humanizer](https://github.com/blader/humanizer) by Siqi Chen (approximately 5,900 stars) was published on 18 January 2026. This repository's deglazer skill (originally named humaniser) was added on 19 February 2026, one month later.

The overlap is substantial and specific:

| Content | blader/humanizer (v2.2.0) | This repo's deglazer (v4.0.0) |
|---|---|---|
| Gallery 825 example | Present, same wording | Present, same wording |
| Catalonia statistics example | Present, same wording | Present, same wording |
| Korattur example | Present, same wording | Present, same wording |
| Temple colour palette example | Present | Present |
| "Copula avoidance" pattern | Present | Present |
| "Significance inflation" pattern | Present | Present |
| "Elegant variation" pattern | Present | Present |
| Overused AI vocabulary word list | Present, same words | Present, same words |
| 24 core patterns | 24 patterns | All 24 present, plus 13 additional |

This repository's version is expanded: 37 patterns versus 24, the severity tier system is new, the corpus-level patterns (7, 8, 16, 17) are additions, and the voice/personality diagnostics (34-37) are additions. But the base content is clearly derived from blader's work.

blader's repository is MIT licensed, so copying and modifying is legally permitted. However, MIT requires that the copyright notice is preserved in derivative works. This repository does not credit blader anywhere. The references section lists Wikipedia and academic papers but omits the most direct source.

### Problem 2: Wikipedia CC BY-SA licence incompatibility

Both this repository and blader's repository derive content from [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing). The specific examples (Gallery 825, Catalonia, Korattur) are real Wikipedia article excerpts used on that page to illustrate AI writing patterns. The word lists and pattern descriptions also overlap with the Wikipedia page's content.

All Wikipedia content is licensed under CC BY-SA 4.0. This licence requires:

1. **Attribution** — the deglazer skill does credit Wikipedia (satisfied)
2. **Share-alike** — derivative works must be released under CC BY-SA or a compatible licence

This repository uses MIT. MIT is not compatible with CC BY-SA's share-alike requirement. If the deglazer skill is a derivative work of the Wikipedia page (and the shared examples and word lists make a strong case that it is), the MIT licence is insufficient.

This applies to blader's repository too, but that does not reduce the risk for this repository.

## Recommendations

**For `/deep-init`**: Safe to offer at work. No changes needed.

**For `/deglazer`**: Do not offer at work in its current form. Options:

1. **Relicense under CC BY-SA 4.0** and add proper attribution to both the Wikipedia page and blader/humanizer. This satisfies both obligations but changes what others can do with the repository.
2. **Rewrite from scratch** using only the academic papers as sources (not Wikipedia, not blader's repository). The underlying research findings are not copyrightable — they are factual observations. But the specific word lists, examples, and editorial framing would need to be replaced entirely.
3. **Remove the deglazer skill** from the repository before offering it at work, and offer only `/deep-init`.

## Limitations of this audit

This audit cannot determine whether the overlap with blader/humanizer constitutes copying in a legal sense versus two people independently drawing from the same Wikipedia source. It also cannot determine whether the Wikipedia-derived content is substantial enough to make the whole file a derivative work under copyright law. A qualified IP lawyer would need to make that determination.
