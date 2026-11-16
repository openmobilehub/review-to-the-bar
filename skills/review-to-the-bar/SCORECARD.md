# The scorecard

Reference for step 8 of [`SKILL.md`](SKILL.md). Every review ends with one: six
scores from 1 to 5, each tied to the validated findings, so the author sees
which part to fix and the person sees at a glance where the change stands.

## The six rubrics

Score each one from the validated findings only. A score below 5 names the
finding that set it.

- **A finding counts against every rubric it affects.** A prompt that can be
  approved with no screen lowers both developer experience and shown is what
  is signed.
- **n/a means the rubric has nothing to judge.** It is never a 5.
- **A finding labelled `issue (blocking)` caps each rubric it affects at 2.**

### 1. Developer experience (gate)

**Measures:** the code a developer writes to use the change, and whether they
can get it right without help: the sample app, the docs, the error messages,
and what happens when a step is skipped.
**Not:** how the library is built inside.
**Judged against:** `BAR.md` §1 and §4, and the larger of the minimal snippet
and what the real caller wrote.
**n/a when:** no developer writes anything new, as in an internal fix.

| Score | Means |
| :-- | :-- |
| 5 | One screen or less, in one place, and a new developer could write it from the docs. Skipping a step is refused with a message that says what to do. |
| 4 | One screen or less. One thing the docs don't say, or one step that is easy to miss but fails loudly. |
| 3 | Short, but in several places, or a step can be skipped silently, or it only makes sense after reading the source. |
| 2 | Past one screen, or the sample app as shipped doesn't work, or a skipped step leads to a wrong result nobody sees. |
| 1 | Hundreds of lines, or a developer can't write it from what is published. |

### 2. Shown is what is signed (gate)

**Measures:** whether a person approving something sees every term that is
then signed or enforced, before they approve.
**Not:** visual design, wording style or translation, unless they hide a term.
**Judged against:** `BAR.md` §2, and the spec's own rule on what must be shown.
**n/a when:** no person approves anything.

| Score | Means |
| :-- | :-- |
| 5 | Every enforced value is visible, ids as well as names. Anything that can't be shown is refused, never left out. |
| 4 | Everything is shown on the normal path. One summary simplifies, but says so and links to the full terms. |
| 3 | Correct on the normal path. Unusual but valid input loses a term, or a summary leaves something out without saying so. |
| 2 | A reachable path shows something false, or leaves out a term that is enforced. |
| 1 | A person can approve terms they were never shown, on the normal path. |

### 3. Correctness and spec (gate)

**Measures:** whether the change does what was asked, works with the other
side, and has no bugs, including security, error handling and bad input.
**Not:** missing tests (evidence) or style (repository rules).
**Judged against:** the spec and its reference implementation, the linked
issue, and otherwise the PR's own description.

| Score | Means |
| :-- | :-- |
| 5 | Matches the spec and the reference implementation. No bug found. |
| 4 | Correct on every path a user reaches. One edge case, unreachable today or failing safely. |
| 3 | Right on the main path. Bugs on valid but unusual input, or it disagrees with the reference implementation in a way the other side tolerates. |
| 2 | Wrong on a path people will reach, or the other side rejects its output. |
| 1 | Wrong on the main path, or unsafe. |

### 4. Evidence

**Measures:** whether the PR's claims can be checked ("tested", "verified",
"fixes", "works with"), and whether each new check has a test that fails
without it.
**Not:** whether the code is right. That is correctness.
**Judged against:** `BAR.md` §3, and the repository's testing rules.

| Score | Means |
| :-- | :-- |
| 5 | Every claim points at a test, script or CI job anyone can run. Each new check has a test that fails when the check is removed. |
| 4 | Every claim can be re-run. One new check has no test of its own. |
| 3 | Tests exist, but some claims have nothing behind them, or several checks are untested. |
| 2 | A claim is false, or a key part has never been run, such as a screen never drawn. |
| 1 | No claim can be checked. |

### 5. Scope and size

**Measures:** whether the change does one thing, and is small enough to review
well.
**Not:** whether the one thing is the right thing. That is correctness.
**Judged against:** `BAR.md` §5, counting source lines without tests, docs,
lockfiles or generated files. For a stacked PR, only its own commits.

| Score | Means |
| :-- | :-- |
| 5 | One purpose, under about 400 source lines. |
| 4 | One purpose, up to about 1,000 lines, or small with one unrelated change. |
| 3 | One purpose over 1,000 lines, or a few unrelated changes mixed in. |
| 2 | Two or three purposes in one change. |
| 1 | Several purposes in one change over 1,000 lines. |

### 6. Repository rules

**Measures:** whether the change follows the target repository's written rules
(its contribution, style, review and agent files) and the pattern its nearest
sibling code sets.
**Not:** rules only your own team holds. Those are judged under the rubric they
belong to, such as the sample-app rule under developer experience.
**Judged against:** the target repository's rule files, read from the base
commit.

| Score | Means |
| :-- | :-- |
| 5 | No breach. |
| 4 | One minor breach of a rule the repository itself often ignores. |
| 3 | A few minor breaches, or one the repository follows consistently. |
| 2 | Breaks a rule the repository states as "must", or one that affects callers, such as compatibility. |
| 1 | Several "must" rules broken. |

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

## Tracking across pull requests

When the person keeps a tracking project (for example a private GitHub project
with one number field each), propose these at hand-over and write them only on
their word, with `gh project item-edit`:

- **Rounds:** the person's reviews on the PR, counted with
  `gh api repos/<owner>/<repo>/pulls/<n>/reviews`, after this one is posted.
- **Score:** the overall score, only when the lenses ran. A change stopped at
  the cold read has no score this round: clear the field rather than keep an
  older round's.
- **Cold read gap:** the person's score minus the agent's.
- **Missed by self-review:** findings in this review that the author's
  self-review packet does not list. Only when the author sent a packet and the
  lenses ran; otherwise clear it.

The fields hold the latest round. Read the trend across pull requests: rounds
falling to 1, scores reaching 4, the gap closing to 0, and missed falling to 0.
