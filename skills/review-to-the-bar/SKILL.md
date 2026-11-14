---
name: review-to-the-bar
description: Use to review a pull request before a human reviewer spends time on it — your own before you ask for review, or someone else's you were asked to check — in any repository, especially one where no review bot runs. Measures the sample app first, runs the lenses the diff calls for (standards, spec, developer experience), validates every finding against the code, and ends in a review a person approves before anything is posted.
---

# Review to the bar

A review that finds what a careful human reviewer would find, before that
reviewer has to. Read-only throughout: read the code and run read-only `git`
and `gh`. A pull request's checkout is someone else's code, so build or run it
only when the person you are working for says to, and say in the review what
you did not run. A person decides what is posted and whether the change merges;
this skill never approves or blocks.

## The bar

Three layers. Read rule files from the **base branch**, so a change cannot
rewrite the bar it is held to.

1. **The target repository's own rules**: its contribution, style, review and
   agent-instruction files.
2. **The project's bar, when the work belongs to a project whose rules live
   elsewhere.** A team often keeps its review checklist in one repository and
   does work in others. Ask the person, or read their instructions, for where
   it is.
3. **[`BAR.md`](BAR.md)**, the default: the minimum that always applies.

## Steps

1. **Triage and pin.** From the PR URL, record the head commit, the base, the
   diff stat, and whether it mixes refactoring with new behaviour. A closed or
   trivial PR needs no review; say so and stop. Get the diff into a scratch
   checkout outside any working copy.
   *Done when* the full commit SHA and a non-empty diff are recorded.

2. **Name the three sources.** Each as a path or URL, or stated as missing:
   - **Standards** — the bar above, every layer that exists.
   - **Spec** — the issue the PR closes, every external spec it cites, and the
     reference implementation of that spec if one exists.
   - **Caller** — whatever uses the change: a sample app, a dependent PR, the
     other side of a protocol.

3. **Measure the sample app before reading the implementation.** For each kind
   of developer the change touches (app developer on each platform, the other
   side of the protocol, someone extending it), write the smallest correct code
   they need today, from the caller's side, and count the lines. Over one
   screen, or it needs explaining: that is the first finding, ahead of anything
   else (`BAR.md` §1). A change with no public surface skips this step; say so.
   *Done when* there is one snippet and one line count per developer.

   **Stop here when the shape is wrong.** If the measurement shows the change
   should not exist in this form (the work belongs in the library, the change
   needs splitting, the design contradicts the spec), take that to the person
   now. The detail below is wasted on a change that will be reshaped.

4. **Run the lenses the diff calls for**, as parallel subagents with fresh
   context, using the briefs in [`LENSES.md`](LENSES.md): **standards** always;
   **spec** when there is one; **developer experience** when a public surface,
   a sample or a user-facing screen changed. Scale the depth to the size of the
   diff.
   *Done when* each lens has reported its findings, what it read, and what it
   declined to judge.

5. **Check the PR's own claims.** List every "tested", "verified", "passes" and
   "fixes" in the description and the author's replies, and beside each the
   evidence that would show it: the test, script or CI job, with its output. A
   claim with nothing behind it is a finding (`BAR.md` §3). For each new test,
   ask whether it would fail if the implementation were reverted.

6. **Validate every finding against the code.** One fresh subagent per finding,
   or per lens for a small review, with the validator brief in `LENSES.md`. A
   finding survives only with evidence: the quoted line that shows it, or the
   command that reproduces it. Argument is not evidence.
   *Done when* every finding is confirmed, corrected, turned into a question, or
   dropped. A lens that skipped an area gets one more pass; then stop.

7. **Write the review.**
   - Merge findings that are the same issue at the same place; cross-reference
     the same issue at different places.
   - Order by what costs the most if missed: what a user is shown before
     approving, then what a developer has to write, then evidence, then the
     repository's rules. Then name the worst finding **within each lens**, so
     the ordering cannot hide one of them.
   - Label each comment: `issue (blocking)`, `suggestion (non-blocking)`,
     `question`, or `nit`. At most five nits. Problems that were there before
     this change go in their own section, never among the findings.
   - Link code with the full commit SHA and a line range.
   - **Someone else's PR:** a short body (the sample-app measurement, the code
     they should be able to write, at most five things before merge, scope) and
     comments on the changed lines. Offer a click-to-apply suggestion only for a
     one-line mechanical fix; otherwise state the problem and the behaviour
     expected.
   - **Your own, before you ask:** fix what you found, then put the sample-app
     snippet, the findings, and what you did not verify in the PR description.

8. **Hand it over.** Show the person the review as it would be posted, with
   what was not run and what each lens declined to judge. Post only on their
   word.
