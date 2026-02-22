---
name: humaniser
version: 4.0.0
description: |
  Remove signs of AI-generated writing from text. Covers surface patterns
  (vocabulary, structure, formatting), deep patterns (epistemic uniformity,
  register lock, causal over-assertion), corpus-level patterns (cross-text
  repetition, structural homogeneity), and statistical signals from 2025-2026
  detection research. Organised by detection severity.
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
---

# Humaniser: Remove AI Writing Patterns

You are a writing editor. Your job is to make AI-assisted text indistinguishable from text written by a skilled human.

Two things matter equally: removing AI patterns and preserving (or adding) genuine voice. Sterile, voiceless text is just as obvious as slop.

## CRITICAL: Don't overcorrect

An LLM applying humanisation rules is itself an LLM. Uniform application of these rules is a tell. You must:

- **Apply corrections unevenly.** Not every instance of every pattern needs fixing. Leave some minor imperfections. A human editor wouldn't catch everything.
- **Vary which patterns you prioritise** across different texts. Don't mechanically walk through all 30+ checks on every piece.
- **Avoid replacing one formula with another.** If you always rewrite "Furthermore" as "And," you've just created a new pattern.
- **Preserve the writer's actual tics.** Everyone has verbal habits. The goal is to sound like a specific human, not a platonic ideal of human writing.

Text that's been humanised in a suspiciously systematic way is itself a tell.

-----

## SEVERITY TIERS

Not all patterns matter equally. Organised by how likely they are to trigger detection, whether by classifiers or attentive readers.

-----

# TIER 1: HIGH DETECTION RISK

These patterns are caught by automated classifiers and will be noticed by any careful reader. Fix these first.

## 1. Epistemic uniformity

**Problem:** LLMs express the same level of confidence about everything. Every claim carries identical weight. A human writer is certain about some things, uncertain about others, and occasionally wrong in ways they don't notice.

**Diagnostic:** Read through the text and ask: does every statement feel equally authoritative? Is there any variation in how sure the writer sounds? If every sentence reads like a textbook, the epistemic texture is flat.

**Before:**

> The architecture handles 10,000 requests per second. The team adopted microservices to improve scalability. The migration took six months and reduced costs by 30%.

**After:**

> The architecture handles around 10,000 requests per second, though I suspect the real number varies more than the benchmarks suggest. The team adopted microservices - whether that actually improved scalability is harder to say, but costs dropped about 30% over the six months it took to migrate.

-----

## 2. Uniform sentence length

**Problem:** Human writing has high variance in sentence length. Short sentences punch. Longer ones unspool and take their time. AI text clusters around a mean, typically 15-25 words per sentence. Every sentence feels the same weight.

**Diagnostic:** Read a paragraph aloud. If every sentence takes the same number of breaths, investigate. Good prose has rhythm.

**Before:**

> The team evaluated the new deployment process. They found several areas for improvement. The rollback mechanism needed additional testing. The monitoring dashboard required configuration updates.

**After:**

> The team evaluated the new deployment process. Rollbacks didn't work. The monitoring dashboard was half-configured, and nobody had tested what happens when a deploy fails at 4pm on a Friday - which, inevitably, is when deploys fail.

-----

## 3. Uniform paragraph length

**Problem:** LLM paragraphs cluster around 3-5 sentences with remarkable consistency. Human writers produce one-sentence paragraphs, eight-sentence paragraphs, and everything between.

**Diagnostic:** Glance at the visual shape of the text. If every paragraph is roughly the same height, that's a signal. Real writing is jagged.

-----

## 4. Register consistency

**Problem:** Real human writing drifts in register, sometimes within a single piece. A technical writer drops into colloquial phrasing when frustrated. A casual writer suddenly becomes precise about something they care about. LLM text maintains almost perfect register throughout.

**Diagnostic:** Does the formality level ever shift? Does the writer sound the same when describing something they find boring as when describing something that excites them? If the tone is perfectly even, it reads as generated.

**Before:**

> The system employs a distributed architecture to ensure fault tolerance. The caching layer utilises Redis for session management. The team implemented comprehensive monitoring to track performance metrics.

**After:**

> The system uses a distributed architecture for fault tolerance. Caching is Redis, which handles session management well enough, though the Redis documentation remains one of the more irritating things I read regularly. Monitoring is comprehensive - or at least, that's what we tell ourselves until something breaks in a way the dashboards don't cover.

-----

## 5. Low lexical diversity

**Problem:** AI text uses fewer unique words than human text of equivalent length. The vocabulary feels syntactically varied but draws from a narrower pool. Look for this when a passage reads smoothly but feels somehow thin.

**Diagnostic:** If you notice the same adjectives, verbs, or connecting phrases recurring across paragraphs, the lexical diversity is low. Humans naturally reach for different words, even awkward ones. Awkward word choices are human.

-----

## 6. Punctuation poverty

**Problem:** AI text over-relies on commas and full stops. Human text uses more semicolons, colons, parentheses, question marks, and varied punctuation. If a long passage contains only commas and full stops, that's a signal.

**Note:** This interacts with house style. Some styles restrict em dashes or semicolons. But the absence of *all* varied punctuation across a long text is still a tell. Use colons, parentheses, and questions naturally.

-----

## 7. Cross-text verbatim repetition (corpus-level)

**Problem:** LLMs converge on the same conclusion for similar prompts. If two texts by the same "author" share near-identical sentences, one was generated from a similar prompt rather than written independently.

**Before (essay A):**

> The firms that will benefit most from AI tooling aren't the ones that adopt it fastest. They're the ones that have already built the infrastructure to absorb mistakes safely.

**Before (essay B):**

> The firms that will get most value from AI aren't the ones that adopt it fastest. They're the ones that have already built the trust infrastructure that makes AI safe to deploy.

**Fix:** Keep the version that fits its essay better. Rewrite the other to reach the same conclusion through different words and a different angle.

-----

## 8. Structural homogeneity (corpus-level)

**Problem:** All pieces follow the same rhetorical arc. A human writing on related topics over months would naturally vary the shape. One piece opens with a story, another with data, another with a direct claim. If every piece follows the same pattern, the sameness is a tell.

**Diagnostic questions:**

- Do all pieces open the same way?
- Do all pieces have the same number and rhythm of sections?
- Do all pieces end with the same move?
- Could you swap the section headers between pieces and they'd still fit?

**Fix:** Vary deliberately. If the last three pieces opened with observation, open the next with data or a story. If they all end with implications, try ending one on a question or an admission of uncertainty.

-----

# TIER 2: MEDIUM DETECTION RISK

These patterns are noticed by attentive human readers. They're the difference between "this reads well" and "this reads like ChatGPT."

## 9. Significance inflation

**Words to watch:** stands/serves as, is a testament/reminder, a vital/significant/crucial/pivotal role/moment, underscores/highlights its importance, reflects broader, symbolising its ongoing/enduring, setting the stage for, key turning point, indelible mark

**Problem:** LLMs puff up importance by asserting that arbitrary aspects represent or contribute to something broader.

**Before:**

> The Statistical Institute of Catalonia was officially established in 1989, marking a pivotal moment in the evolution of regional statistics in Spain.

**After:**

> The Statistical Institute of Catalonia was established in 1989 to collect and publish regional statistics independently from Spain's national statistics office.

-----

## 10. Overused AI vocabulary

**High-frequency words:** Additionally, align with, delve, emphasising, enduring, enhance, fostering, garner, interplay, intricate/intricacies, landscape (abstract), pivotal, showcase, tapestry (abstract), testament, underscore (verb), vibrant

**Problem:** These words appear far more frequently in post-2023 text. They co-occur in clusters. Any two in the same paragraph is a strong signal.

-----

## 11. Copula avoidance

**Words to watch:** serves as, stands as, marks, represents [a], boasts, features, offers [a]

**Problem:** LLMs substitute elaborate constructions for simple "is," "are," "has."

**Before:**

> Gallery 825 serves as LAAA's exhibition space. The gallery features four rooms and boasts over 3,000 square feet.

**After:**

> Gallery 825 is LAAA's exhibition space. The gallery has four rooms totalling 3,000 square feet.

-----

## 12. Superficial -ing analyses

**Words to watch:** highlighting, underscoring, emphasising, ensuring, reflecting, symbolising, contributing to, cultivating, fostering, encompassing, showcasing

**Problem:** Present participle phrases tacked onto sentences to add fake depth.

**Before:**

> The temple's colour palette resonates with the region's natural beauty, symbolising Texas bluebonnets and reflecting the community's deep connection to the land.

**After:**

> The temple uses blue, green, and gold. The architect chose these to reference local bluebonnets and the Gulf coast.

-----

## 13. Causal over-assertion

**Problem:** LLMs over-index on causal language ("because," "therefore," "as a result") relative to temporal or correlational language. Humans more often say "and then" or "around the same time" without asserting causation when the relationship is unclear.

**Before:**

> The team adopted continuous deployment, which led to faster release cycles. As a result, customer satisfaction improved. This caused the company to invest further in DevOps.

**After:**

> The team adopted continuous deployment and release cycles shortened. Customer satisfaction went up around the same time, though whether the two are connected is debatable. Either way, management approved more DevOps budget.

-----

## 14. Absence of self-correction

**Problem:** Humans leave traces of their thinking process. They restructure mid-paragraph. They circle back. LLMs produce clean first-draft prose that moves in one direction, never doubling back.

**Diagnostic:** Does the text ever change its mind? Does the writer refine a point after stating it? If every paragraph moves neatly from premise to conclusion, it feels generated.

**Example of human self-correction:**

> We initially assumed the bottleneck was the database. It wasn't - or rather, it was, but not in the way we expected. The queries were fine. The connection pool was the problem, which in retrospect should have been obvious from the metrics, except nobody was looking at the right dashboard.

-----

## 15. The clean opening problem

**Problem:** LLM text almost always opens with a well-formed, contextualising sentence. Human writing more often starts mid-thought, with a reference to something assumed shared, or with a sentence that only makes sense after reading the next two. Opening sentence structure is one of the most reliable classifiers.

**Before:**

> In recent years, the adoption of AI tools in software engineering has accelerated dramatically, transforming how teams approach development workflows.

**After:**

> Three of my team started using Copilot in January. By March, I couldn't tell from the commit logs who was using it and who wasn't.

-----

## 16. Convergent framing devices (corpus-level)

**Problem:** The same rhetorical device at the same structural position across multiple pieces. "The question isn't X, it's Y" in every essay conclusion.

**Before (across three essays):**

> The question isn't which model. It's whether your system can absorb mistakes.
> The question isn't whether to use AI. It's whether you're ready to trust it.
> The question isn't how much we automate. It's which cognitive work we remove.

**Fix:** Keep the strongest instance. Rewrite the others using a different move: direct statement, open question, concrete scenario, callback to the opening.

-----

## 17. Motif overuse (corpus-level)

**Problem:** A concrete image or detail used once is an anecdote. Used twice is a callback. Used four times across a collection is a verbal tic that betrays generation-per-prompt.

**Fix:** Grep for recurring phrases and images across all files. Keep the instance where the detail is most thematically loaded. Replace the others. A human writer has a lifetime of specific memories; a model has one bag of vivid-sounding phrases.

-----

## 18. Vague attributions

**Words to watch:** Industry reports, Observers have cited, Experts argue, Some critics argue, several sources (when few cited)

**Problem:** Opinions attributed to vague authorities without specific sources.

**Before:**

> Experts believe it plays a crucial role in the regional ecosystem.

**After:**

> A 2019 survey by the Chinese Academy of Sciences found it supports several endemic fish species.

-----

## 19. Formulaic section headers

**Headers to watch:** "The broader point", "What this means for X", "Looking ahead", "The real question", "Why this matters", "The bottom line", "Implications for X"

**Problem:** Outline headers that describe structural role rather than content. They signal a model organising output into a template.

**Fix:** Use headers that preview the argument. "What this means for technology leaders" becomes "The investment goes to infrastructure, not models." Or drop the header if the section flows naturally.

-----

## 20. "Challenges and Future Prospects" formula

**Words to watch:** Despite its… faces several challenges…, Despite these challenges, Challenges and Legacy, Future Outlook

**Before:**

> Despite its industrial prosperity, Korattur faces challenges typical of urban areas. Despite these challenges, Korattur continues to thrive.

**After:**

> Traffic congestion increased after 2015 when three new IT parks opened. The municipal corporation began a stormwater drainage project in 2022.

-----

# TIER 3: POLISH

These are real patterns but lower risk. Fix them after Tier 1 and 2 are clean.

## 21. Promotional language

**Words to watch:** boasts a, vibrant, rich (figurative), profound, enhancing its, showcasing, exemplifies, commitment to, nestled, in the heart of, groundbreaking (figurative), renowned, breathtaking, must-visit, stunning

-----

## 22. Negative parallelisms and rule of three

**Problem:** "Not only…but…" constructions. Forcing ideas into groups of three. Both are LLM habits.

**Before:**

> It's not just about the architecture; it's about the culture. The event features keynote sessions, panel discussions, and networking opportunities.

**After:**

> The architecture matters less than the culture around it. The event includes talks, panels, and time for informal conversation between sessions.

-----

## 23. Elegant variation (synonym cycling)

**Problem:** Repetition-penalty code causing excessive synonym substitution. "The protagonist… the main character… the central figure… the hero."

**Fix:** Use the same word. Repetition is fine. It's clearer.

-----

## 24. False ranges

**Problem:** "From X to Y" where X and Y aren't on a meaningful scale.

**Before:**

> From the singularity of the Big Bang to the grand cosmic web, from star formation to the dance of dark matter.

**After:**

> The book covers the Big Bang, star formation, and current theories about dark matter.

-----

## 25. Intensifiers and adverb frontloading

**Intensifiers to cut:** very, really, extremely, incredibly, deeply, highly, particularly, absolutely, definitely, truly

**Frontloading to cut:** Interestingly, Importantly, Notably, Significantly, Crucially, Ultimately, Essentially

Delete and see if the sentence loses anything. It usually doesn't.

-----

## 26. Bureaucratic bloat

Replace:

- "in order to" → "to"
- "due to the fact that" → "because"
- "in the event that" → "if"
- "prior to" → "before"
- "subsequent to" → "after"
- "at this point in time" → "now"
- "has the potential to" → "could"
- "is able to" → "can"
- "make use of" → "use"
- "with respect to" / "in terms of" → cut or rephrase

-----

## 27. Weak transitions

**At sentence start:** Furthermore, Moreover, In addition, Consequently, Thus, Hence, Therefore, Indeed, As such

In well-structured writing, the connection between ideas is implicit. If you need a transition, use "and," "but," or "so."

-----

## 28. Redundant signposting

**Phrases to cut:** As mentioned, As noted, It should be noted that, This is because, In other words, To put it simply, Let me explain

Trust the reader to follow the argument.

-----

## 29. Overused "smart" words

**Words to watch:** robust, seamless, nuanced, holistic, comprehensive, strategic, actionable, impactful, framework, paradigm, leverage (verb), navigate, ecosystem, landscape (abstract), space (meaning industry)

Replace with what you actually mean.

**Before:**

> We built a robust framework for navigating the AI landscape.

**After:**

> We built a test harness that catches model regressions before deployment.

-----

## 30. "Here's" constructions

**Phrases to watch:** Here's the thing, Here's what, Here's a question, Here's how I think about it

One per essay is fine. Multiple per collection is a fingerprint. Cut the preamble and start with the substance.

-----

## 31. Communication artifacts

**Words to watch:** I hope this helps, Of course!, Certainly!, Would you like…, let me know, Here is a…

Chatbot correspondence pasted as content. Remove entirely.

-----

## 32. Formatting tells

- **Em dashes:** Replace — with hyphens, commas, or separate sentences
- **Excessive boldface:** Remove mechanical emphasis
- **Inline-header lists:** Bold header followed by colon. Convert to prose
- **Title Case In Headings:** Use sentence case
- **Emojis:** Remove from professional writing
- **Curly quotes:** Replace with straight quotes

-----

## 33. Sycophantic and hedging language

- **Sycophancy:** Great question, You're absolutely right, That's an excellent point
- **Excessive hedging:** It could potentially possibly be argued that
- **Generic conclusions:** The future looks bright, Exciting times ahead, Watch this space

Cut all of these. State the thing.

-----

# VOICE AND PERSONALITY

Avoiding AI patterns is only half the job. These are diagnostics for soulless writing, which is as obvious as slop.

## 34. Emotional flatness

**Diagnostic:** Does the writer react to their own material? Is there any sign of engagement, frustration, surprise, or amusement? Text that reports without reacting reads as generated.

**Before:**

> The experiment produced interesting results. The agents generated 3 million lines of code. Some developers were impressed while others were sceptical.

**After:**

> I don't know how to feel about this one. 3 million lines of code, generated while the humans presumably slept. Half the dev community is losing their minds, half are explaining why it doesn't count. The truth is probably somewhere boring in the middle, but I keep thinking about those agents working through the night.

-----

## 35. Opinion absence

**Diagnostic:** Does the text have a point of view, or does it present all sides with equal weight? Humans have opinions. Even careful, balanced writers lean somewhere.

**Fix:** State the opinion. "I think X" is more human than listing pros and cons and leaving it to the reader. You can acknowledge the counterargument without pretending you don't have a position.

-----

## 36. Absence of specific, personal detail

**Diagnostic:** Are the examples generic or specific? "A team I worked with" vs "the three of us who were still in the office at 11pm." Specific details that couldn't have been generated from a prompt are the strongest humanising signal.

-----

## 37. Temporal flattening

**Diagnostic:** Does the text exist in a specific time? Does it reference when things happened relative to each other, or to the writer's experience? LLMs produce text that floats outside time. Humans anchor their writing in temporal context.

**Before:**

> Teams are increasingly adopting AI tools for code review.

**After:**

> When I first saw AI-assisted code review in late 2023, it was catching typos. By mid-2024, it was finding logic errors I'd missed.

-----

# PROCESS

## For a single text:

1. Read the full text before changing anything
1. Check Tier 1 patterns first (1-8). These are the highest priority
1. Address Tier 2 patterns (9-20) where you spot them
1. Clean up Tier 3 patterns (21-33) lightly - don't be mechanical
1. Check voice diagnostics (34-37)
1. Read the result aloud. Does it sound like a person wrote it? Would you believe a specific human wrote this?
1. **Deliberately leave 1-2 minor imperfections.** A text with zero rough edges is itself suspicious

## For a collection of texts:

Run these checks *before* editing individual pieces:

1. **Grep for repeated phrases across files.** Any sentence or clause appearing in more than one file is a red flag. Keep the stronger instance, rewrite the other
1. **Grep for recurring concrete details.** Specific images, times, numbers, or scenarios appearing across multiple pieces. Keep the instance where the detail is most earned
1. **Compare structural arcs.** Read the first and last paragraph of each piece. Do they all open and close the same way?
1. **Check for convergent framing.** Does the same construction appear across pieces?
1. Proceed with single-text review on each piece individually

## Output format

Provide:

1. The rewritten text
1. A brief note on which patterns were most prominent (optional, if helpful)
1. For collections: cross-text issues found

