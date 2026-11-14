# The scorecard

Reference for step 8 of [`SKILL.md`](SKILL.md). Every review ends with one: six
scores from 1 to 5, each tied to the validated findings, so the author sees
which part to fix and the person sees at a glance where the change stands.

## The six rubrics

Score each from the validated findings only. A score below 5 names the finding
that set it. Use 2 and 4 for what falls between the anchors.

| Rubric | 5 | 3 | 1 |
| :-- | :-- | :-- | :-- |
| **Developer experience** (gate) | The sample app fits one screen and reads without opening the source. | It works, but is past one screen or needs explaining. | Hundreds of lines, or a developer cannot write it. |
| **Shown is what is signed** (gate) | Every enforced value is visible; anything unreadable is refused. | Right on the normal path; gaps on unusual input. | A person can approve terms they were not shown. |
| **Correctness and spec** (gate) | Matches the spec and its reference implementation; no bug found. | Right on the main path; bugs at the edges. | Wrong behaviour on a path people will reach. |
| **Evidence** | Every claim can be re-run; each new check has a test that fails without it. | Tests exist; some claims have nothing behind them. | Claims with no way to check them. |
| **Scope and size** | One purpose, under about 400 source lines. | One purpose but large, or small with unrelated changes. | Several purposes in one change over 1,000 lines. |
| **Repository rules** | No breach. | A few minor ones. | Breaks a rule the repository states as a must. |

- Measure developer experience on the larger of the minimal snippet and the
  real caller (`BAR.md` §1).
- A rubric with nothing to judge is **n/a**, never a 5: "shown is what is
  signed" when no person approves anything, "developer experience" when no
  developer writes anything new.
- A finding labelled `issue (blocking)` caps its rubric at 2.

## The overall score

The lowest of the gates that apply. Never an average: an average hides which
part is the problem.

- **4 or above on every gate:** ready for a person's review.
- **Below 4 on any gate:** goes back, with that rubric's finding first.
- **Any finding labelled `issue (blocking)`, in any rubric, holds the overall
  score at 3 or below.** A change with something to fix before merge is not a
  4, even when the fix is one line.

Otherwise evidence, scope and rules are reported beside the overall score and
do not lower it. They tell the author what to fix next.

## The cold read

Reported on its own line, never folded in: the person's score, and yours once
theirs is given ([`HUMAN-BAR.md`](HUMAN-BAR.md)). Below 4, the change goes back
whatever the scorecard says.

## How it is written

At the end of the review to the author, and in the hand-over:

```
| Rubric | Score | Set by |
| :-- | :-- | :-- |
| Developer experience | 1 | 44 lines minimum; the real caller wrote 381 + 246 |
| Shown is what is signed | 2 | rows dropped from the summary without notice |
| Correctness and spec | 4 | one edge case in the ISO branch |
| Evidence | 2 | the interop run cannot be re-run |
| Scope and size | 1 | 1,307 source lines, four purposes |
| Repository rules | 3 | two style-guide nits |
| **Overall** | **1** | lowest gate: developer experience |
Cold read: pending
```

- "Set by" is one finding, in a few words, that the review already states.
- Validators check the scores with the labels: does each score follow from its
  anchor and its finding?
- The scores are yours, proposed from the evidence. The person can change any
  of them at hand-over, and theirs stand.
- The scorecard goes to the author. It is left out of an upstream draft unless
  the person asks for it there.
