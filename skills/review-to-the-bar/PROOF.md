# Proof, for your own change

Reference for [`SKILL.md`](SKILL.md), "Your own change". Before you hand a change
to your reviewer, prove each thing it claims. It is your code, so build it, run
it, and keep what it shows. Checking happens while you build, when a fix costs
minutes, not after review, when it costs a round trip and your reviewer's time.

The rest of this skill reads code without running it. Proof is what reading
cannot give: what the screen shows, whether it compiles, whether a test fails.

## What counts as proof

Each claim the change makes, in the description, the commit message or the
docs, gets its proof. All of it at the commit you hand over, on an unmodified
tree.

| The claim | Its proof |
| :-- | :-- |
| A person sees this before approving | A screenshot of the sample app as it ships, default settings included, on every platform the change draws on. One more with each setup step left out, showing what happens then. |
| A developer writes this much code | The sample-app code, compiled, with its line count. |
| A check refuses bad input | The test, and its run with the check removed: it has to fail. |
| It works with the other side | A script anyone can re-run against the other side's reference implementation, and its output. |
| Fixes #N | The steps from the issue, run before and after the change. |
| Tested, passes | The command, and its output at this commit. |

A screenshot from a modified tree, an older commit, or a different sample app
is not proof of this change.

## What you could not prove

Say so by name, with the reason: "the iPhone screen is not drawn; I have no
Xcode 27". A claim with no proof and no reason is a finding against your own
change, as it would be in anyone else's (`BAR.md` §3), and it holds the
evidence score below 4 ([`SCORECARD.md`](SCORECARD.md)).

## Where it goes

With the cold-read packet and the scorecard, by the same rule
([`WRITING.md`](WRITING.md), "For your own change"): in your own team's
repository, in the PR description; in someone else's, privately to your
reviewer. Screenshots and runs that the target repository would want, such as a
golden image or a test, go in the change itself.
