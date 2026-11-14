# The human judgement bar

Reference for the cold read in [`SKILL.md`](SKILL.md). A reviewer agent can check
rules and code, but it cannot tell whether a person understands a feature: it
reads everything as clear. Only a person reading cold can answer that, so this
part is theirs and cannot be delegated to an agent.

It applies when a change has something a developer uses: a public API, a sample,
or documentation. A change with none of those skips it.

## The cold read

Show the person only what a new developer would see, and nothing else: the
README or getting-started section for the feature, and the sample-app code from
step 3. No findings, no diff, no summary of yours, so their judgement is their
own. It should take two or three minutes. If it takes longer, that is already an
answer.

The packet is the repository's words, not yours. Copy the docs as they stand at
the commit; where there are none, say so and copy the doc comment a developer
would find. Show the code the docs and the sample app give. A step the docs
leave out stays out of the packet, and you add no comments saying where code
goes: finding that out is what the read measures.

Ask three questions, in these words:

1. **What is this for?** Say the use case in one sentence.
2. **Is it simple?** Is it a few lines of code?
3. **Where does it go?** Is it clear where this code belongs in an app?

Then ask for one score.

## The scale

| Score | Meaning |
| :-- | :-- |
| 5 | I could add this to an app now, without asking anyone. |
| 4 | I understand what it is for and where it goes; one small thing I had to look up. |
| 3 | I understand it, but I had to work it out. |
| 2 | I can tell roughly what it is for, but not how to use it or where it goes. |
| 1 | I cannot tell what this is for. |

**4 passes.** Below 4, the change goes back, whatever the detailed findings say,
with the person's own words on what was unclear as the first item of the review.

## The agent's score

Before asking the person, and before any lens runs, answer the same three
questions yourself from the packet alone, against the same scale, and write the score down with one line of reasoning per question.
Show it only after the person has given theirs.

- The agent's score never passes or fails anything. Self-scores are not reliable
  enough to gate on.
- The **gap** is the finding. When the agent says 4 and the person says 2, the
  agent could not see what is confusing; say so, and treat what the person
  stumbled on as the top item.
- Record both scores in the review, so the gap can be tracked from one change to
  the next.

## Who is "the person"

Someone who did not write the change.

- **Reviewing someone else's PR:** the person you are working for does the cold
  read.
- **Reviewing your own before you ask:** you are not a cold reader. Prepare the
  cold-read packet (the README section, the sample-app code, the three questions,
  the scale, and your own score, folded so it is not seen first) and put it at
  the top of the PR description for the reviewer to do in two minutes.
