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
  - the developer cannot write the sample app, or it is far past one screen;
  - a claim the change makes is false.

  A shipped path has to reach it: a problem in code nothing can call is a
  suggestion. Where the repository's rules grade severity, use theirs.
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
- Otherwise: one comment per root cause, and about seven line comments at most.
  Choose by the order above.
- Everything else that is true goes in an **Also noticed** list for the person
  at hand-over, with the pre-existing problems, what was declined and what was
  dropped. They decide whether any of it is posted.
- In someone else's repository, cite their rules, the spec, or the code. Your
  own project's bar is not an argument there.
- The change you ask for has to satisfy the rule you cite. Check each remedy
  against its rule before you write it down.

## The body, for someone else's change

Short: about 200 to 300 words, plus code.

1. One or two sentences: what the change gets right, and the verdict in plain
   words.
2. The sample-app measurement: the snippet, its line count, and the code they
   should be able to write when that differs. For a change that is already
   right, one sentence.
3. **Before merge**: the blocking items, at most five. Group related ones into
   one item. More than five distinct blockers means the change needs reshaping
   first; say that in place of a longer list.
4. One line pointing at the non-blocking comments, with their count.
5. **Scope**, when there is something to say: unrelated changes, size, what
   belongs in another PR.
6. **Not verified**: what was not built or run, and which test outcomes are
   traces, in three lines at most.
7. Both cold-read scores, when there was a cold read.

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

Fix what you found. Then the top of the PR description carries, in this order:
the cold-read packet (`HUMAN-BAR.md`), the findings and what you did about
each, and what you did not verify.
