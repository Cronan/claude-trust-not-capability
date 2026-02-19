# LinkedIn post: humaniser launch

## How to post

1. Open LinkedIn
2. Tap "Start a post"
3. Paste everything between the --- lines below
4. Post it
5. Immediately comment on your own post with the text from the "First comment" section

---

## Post text (copy everything below this line)

I built a writing tool that removes AI slop from text.

Not because AI-assisted writing is bad. Because most of it sounds like this:

--

BEFORE:

"I'm thrilled to share that our team has been on an incredible journey leveraging AI to transform our development workflows. The results have been nothing short of remarkable. Furthermore, fostering a culture of innovation is pivotal. The interplay between human creativity and AI capabilities creates a rich tapestry of possibilities that truly showcases the potential of modern engineering teams. The future of software development has never looked more exciting. Agree?"

AFTER:

"We've been using AI tools on the team for about six months. Some of it's working. The code review stuff caught a race condition last week that three of us had missed, which was humbling and useful in roughly equal measure. The code generation is more mixed. It's fast, but fast and wrong is just technical debt with better marketing."

--

BEFORE:

"In today's rapidly evolving technological landscape, the adoption of AI-powered tools is no longer optional -- it's essential. Organizations that fail to embrace this paradigm shift risk falling behind. Here are 5 key strategies for successfully implementing AI in your workflow."

AFTER:

"Most AI tool adoption I've seen follows the same pattern: someone installs it, uses it enthusiastically for a week, hits a limitation that annoys them, and quietly stops. The teams where it actually sticks are the ones that figured out which specific tasks it's good at and stopped trying to use it for everything else."

--

The tool is a Claude Code skill called /humaniser. It covers 37 patterns organised by detection severity -- from the structural tells that classifiers catch (epistemic uniformity, sentence length clustering, register lock) down to the vocabulary tics we all recognise but couldn't name (copula avoidance, significance inflation, the word "delve").

It draws on:

- The Wikipedia "Signs of AI writing" page, maintained by WikiProject AI Cleanup
- "Linguistic Characteristics of AI-Generated Text" (arXiv, 2025)
- "The Disappearing Author" (Researchleap, 2025)
- Liang et al., "Monitoring AI-Modified Content at Scale" (2025)
- Mitchell et al.'s DetectGPT follow-up work
- Gehrmann et al.'s GLTR lineage on uniform token probability

Key finding across all of them: as models get better, vocabulary tells fade, but structural patterns persist. The strongest tells are now about texture, not word choice.

I built it as part of a wider project on how teams should work with AI coding agents. The thesis: AI is a force multiplier only if your systems are designed to absorb mistakes cheaply. Turns out that applies to writing as much as code.

The skill is open source. Link in comments.

---

## First comment (post this as a comment on your own post)

The /humaniser skill is here: https://github.com/Cronan/claude-trust-not-capability/blob/main/skills/humaniser/SKILL.md

It's a Claude Code skill. Copy the directory into your repo's .claude/skills/ and run /humaniser on any text. MIT licensed.

---

## Carousel option (optional, more effort, more reach)

A carousel is just a PDF where each page becomes a swipeable slide.

To make one:
1. Open Canva, Google Slides, or PowerPoint
2. Make one slide per page (see layout below)
3. Export as PDF
4. In LinkedIn, tap "Start a post", then tap the document icon (page with folded corner)
5. Upload the PDF

Slide layout:
- Slide 1: "I built a tool that removes AI slop from text."
- Slide 2: BEFORE example 1 (red heading)
- Slide 3: AFTER example 1 (green heading)
- Slide 4: BEFORE example 2
- Slide 5: AFTER example 2
- Slide 6: "37 patterns. 3 severity tiers. Based on 2025-2026 detection research."
- Slide 7: Sources list
- Slide 8: "Open source. Link in comments."

Recommendation: post the text version first. If it gets traction, make the carousel and repost a week later.
