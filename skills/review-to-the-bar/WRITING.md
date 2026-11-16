# Writing the review

Reference for step 8 of [`SKILL.md`](SKILL.md).

## Labels

Every comment carries one.

- **`issue (blocking)`** — must change before merge. Use it only when one of
  these holds:
  - a rule in the bar that governs this repository says it blocks;
  - a person would be shown something false, or would approve terms they were
    not shown;
  - the behaviour is wrong, or will not work with the other side;
  - the developer cannot write the sample app, or it is far past one screen,
    measured on the larger of the minimal snippet and the real caller;
  - a claim about behaviour or testing, in the description, the commit message
    or the public documentation, is false or cannot be re-run, and a reviewer
    would rely on it. A wrong sentence in a code comment is a suggestion.

  A shipped path has to reach it: a problem in code nothing can call, or that
  no shipped type reaches, is a suggestion. A call a developer can make through
  the public API is a path. A trace you could not run is a question until a
  validator confirms it from the code. A doc comment on a declaration is a code
  comment. Label each finding by itself, also inside a grouped comment.
  A stacked PR that fixes a blocking item changes the request ("land #N with
  this, or first"), not the label; the finding is still stated. Where the
  repository's rules grade severity, use theirs.
- **`suggestion (non-blocking)`** — true and worth doing, and the change can
  merge without it.
- **`question`** — you could neither confirm nor refute it from the code, or the
  answer is the author's to give.
- **`nit`** — small. At most five in a review.

Problems that were there before this change are **pre-existing**: never among
the findings, and not posted unless this change is what makes one reachable for
the first time. Say so when it does.

## Order

Blocking first. Within each label, by what costs the most if missed: what a
user is shown before approving, then what a developer has to write, then
evidence, then the repository's rules. Then name the worst finding within each
lens, so the ordering cannot hide one of them.

Before you finish, read the requests together. Two that pull against each other
("ship the screen here" and "split this PR") need one sentence saying which
comes first.

## How much to post

A review is read by a busy person. What you post is a selection, not the
record.

- When the target repository has its own review format or limits, use them.
- Otherwise: one comment per blocking root cause, about 120 words each, with
  the simplest change that fixes it. Then up to five non-blocking comments,
  the target repository's "must" rules first. Documentation fixes share one
  comment.
- Everything else that is true goes in an **Also noticed** list for the person
  at hand-over, with the pre-existing problems, what was declined and what was
  dropped. They decide whether any of it is sent. When that list is not empty,
  the body ends with one line giving its count and naming the three most
  costly.
- The change you ask for has to satisfy the rule you cite. Check each remedy
  against its rule before you write it down.

## The upstream draft

Only when the person asks to post in someone else's repository. It is a second
text, made from the review to the author.

- Keep every blocking item whose ground upstream accepts: their rules, the
  spec, wrong behaviour, or the code's own stated intent.
- Restate the rest from evidence in their repository: the measurement, the
  pinned caller's line count, how the repository's own sibling does it. An item
  that still has no such ground is posted as a suggestion.
- Cite their rules, the spec, or the code. Your own project's rule files are
  not cited there.

## The body, for someone else's change

Short: about 200 to 300 words, plus code.

1. One or two sentences: what the change gets right, and the verdict in plain
   words.
2. The sample-app measurement: the snippet, its line count, the real caller's,
   and the code they should be able to write when that differs. When it is past
   one screen, say so as the verdict's first sentence ("the API is not
   finished"), give both numbers, the minimum and what the real caller wrote,
   and make it the first item of **Before merge**. For a change that is
   already right, one sentence.
3. **Before merge**: the blocking items, always listed, at most five. Group
   related ones into one item that points at their comments. When the change
   also needs splitting or reshaping, say so in one sentence beside the list,
   never in place of it.
4. One line pointing at the non-blocking comments, with their count.
5. **Scope**, when there is something to say: unrelated changes, size, what
   belongs in another PR.
6. **Not verified**: what was not built or run, and which test outcomes are
   traces, in three lines at most.
7. The scorecard ([`SCORECARD.md`](SCORECARD.md)), outside the word count.
8. Both cold-read scores, when the person has given theirs. A pending cold read
   and your own score alone stay in the hand-over.

What each lens declined to judge, and what was dropped in validation, go to the
person at hand-over with the **Also noticed** list. They are not part of what
is posted.

## Where each comment goes

- **On a changed line** when the evidence is there. Link code with the full
  commit SHA and a line range, in the repository the PR targets.
- **Evidence on an unchanged line**: comment on the changed line that creates
  the obligation, and cite the other.
- **About the description, the commit message or the PR as a whole**: in the
  body. There is no line to attach it to.
- **A click-to-apply suggestion** for a one-line mechanical fix, and only for
  that. Otherwise state the problem and the behaviour expected, and leave the
  fix to the author.

## For your own change, before you ask

Fix what you found. Then give your reviewer, in this order: the cold-read
packet (`HUMAN-BAR.md`), the scorecard, the proof for each claim
(`PROOF.md`), the findings and what you did about each, and what you could not
prove, with the reason.

- **In your own team's repository:** at the top of the PR description.
- **In someone else's repository:** privately, to your own reviewer: a message,
  or a comment on your team's board card. The PR description there carries
  only what that repository's template asks for. Scores and the cold-read
  packet are your team's working notes, not something to publish upstream.

You always see your own scorecard. Who else sees it is your reviewer's call.
