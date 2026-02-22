# Code Review as a Team Practice

Code review looked different ten years ago. Formal inspections, printed diffs, a room with a whiteboard. What we do now barely resembles that, which is mostly an improvement and partly a loss -- the old way forced you to actually read the code instead of skimming a PR on your phone between meetings.

The reviews that teach me something tend to share a few traits. The reviewer names a specific concern: "this retry logic won't back off if the upstream returns 503" rather than "consider error handling." They focus on design choices and edge cases, not whether you used `camelCase` or `snake_case`. And they happen fast. A review sitting open for three days isn't a review; it's a bottleneck with a green icon.

Tooling helps more than I expected it to and less than vendors claim. Automated linting and CI checks are genuinely useful -- they take the style arguments off the table so humans can argue about things that matter. Inline commenting beats email threads. But I've seen teams spend weeks customizing review workflows in ways that produced no measurable change in defect rates. The tool is not the practice.

What makes code review actually work, and I mean work as a learning mechanism rather than a gate, is that the people involved have to respect each other's judgment. An author who assumes every comment is a nitpick will resist useful feedback. A reviewer who assumes the author is careless will waste time on trivia. This takes hundreds of interactions to build and about three bad ones to damage. I watched it happen on a team last year: one senior engineer's habit of leaving sarcastic review comments drove two mid-level engineers to start submitting minimal PRs specifically to minimize his surface area for criticism. The code got worse because the reviews felt hostile.

None of this is fast. A team that commits to doing reviews well should expect months before the habit feels natural rather than performative.
