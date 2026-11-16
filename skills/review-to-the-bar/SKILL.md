---
name: review-to-the-bar
description: Use to review a pull request before a human reviewer spends time on it — your own before you ask for review, or someone else's you were asked to check — in any repository, especially one where no review bot runs. Measures the sample app first, asks a person for a two-minute cold read, runs the lenses the diff calls for (standards, spec and correctness, developer experience), validates every finding against the code, and ends in a review a person approves before anything is posted.
---

# Review to the bar

A review that finds what a careful human reviewer would find, before that
reviewer has to. Read-only throughout: read the code, and run read-only `git`
and `gh` (`gh pr checks` included). A pull request's checkout is someone else's
code, so build or run it only when the person you are working for says to, and
say in the review what you did not run. Your own change is the exception: you
build and run it to prove what it claims ([`PROOF.md`](PROOF.md)). A person
decides what is posted and whether the change merges; this skill never approves
or merges.

## Which review this is

Settle these two before step 1; they change what you produce.

- **Whose change.** *Your own* when the PR's author is the account you run as,
  or the person says so. Otherwise *someone else's*.
- **A teammate's PR in someone else's repository has two outputs, in separate
  steps.** First **the review to the author**: private, held to the full bar.
  This is what the steps below produce. Then, only when the person asks, **the
  upstream draft** ([`WRITING.md`](WRITING.md), "The upstream draft"). Never
  lower a label in the first because of what the second can cite.
- **How big.** Count changed lines of source, leaving out tests, docs, lockfiles
  and generated files. For a PR stacked on another open PR, count and review
  only its own commits; a problem in the lower PR's code that this one first
  puts in front of a person or a developer is a finding here, and the rest go
  to the hand-over for the lower PR's review.
  - **Small**, under about 100: you are the one reviewer. Run every brief that
    applies yourself, in turn, then give one fresh validator your findings,
    your step 3 measurement and your "clean" statements. Post the blocking
    items and at most three others.
  - **Medium**, about 100 to 1,000: one subagent per lens; validators as in
    step 6.
  - **Large**, over about 1,000, or several purposes in one change: "split
    this" is a finding in its own right. Review it by area, one lens pass each.

## The bar

Three layers. Read rule files, and any spec the change itself edits, from the
**base commit the PR records**, so a change cannot rewrite the bar it is held
to. List the spec edits as decisions the change makes.

1. **The target repository's own rules**: its contribution, style, review and
   agent-instruction files. These govern form, compatibility and severity
   there.
2. **The project's bar, when the work belongs to a project whose rules live
   elsewhere.** Ask the person, or read their instructions, for where it is.
3. **[`BAR.md`](BAR.md)**, the default: the minimum that always applies.

Every layer decides the verdict in the review to the author. `BAR.md` is this
skill's minimum, not "your project's bar": a finding under it keeps its label
wherever the code lives.

## Steps

1. **Triage and pin.** Record the head commit the PR shows now, the base commit,
   the diff stat, and whether it mixes refactoring with new behaviour. If you
   were given a different commit, say which you are reviewing and that the
   description may describe another. A closed PR, or one with no behaviour a
   caller or the other side could observe (a typo, a comment, formatting),
   needs no review: say so and stop. Save the diff, and read whole files by
   commit SHA.
   *Done when* the full commit SHA and a non-empty diff are recorded.

2. **Name the sources.** Each as a path or URL, or stated as missing:
   - **Standards** — the bar above, every layer that exists.
   - **Spec** — the issue the PR closes (its body makes claims too), every
     external spec it cites, and that spec's reference implementation if there
     is one.
   - **Caller** — whatever uses the change: a sample app, a dependent PR, the
     other side of a protocol, and any open PR stacked on this one or by the
     same author on the same issue. Read each one's diff and description now,
     and write one line per PR saying what it adds. Pin each caller to a
     commit as well; a caller that moves under the review hides what it really
     had to write.

   Write these, the pin, the diff command, the PR title, description and commit
   message, and every limit the person set, into **one context file**. Every
   subagent reads it.

3. **Measure the sample app.** Read only what a caller sees: public signatures,
   docs, the README, an existing sample. For each kind of developer the change
   touches, write the smallest correct code they need today, starting from an
   app on the library's defaults, and count the lines
   as a sample app would format them; for a developer who writes nothing new,
   one sentence saying so. Beside each minimal snippet, give the number of
   lines the real caller wrote at its pinned commit; when the two differ, the
   larger is what developers will copy. For a fix, measure before and after.
   On a later round of the same PR, measure the previous round's head too: when
   the caller's code grew and the developer gained nothing they asked for, that
   is a finding, and developer experience cannot score 4 on your own change.
   Say the snippets are uncompiled when you could not build. Then trace each
   sample to the screen: as the sample app ships, with its default settings,
   what does a person see, and what happens when each statement of the snippet
   is left out? Over one screen, or needing explanation, is the first finding
   (`BAR.md` §1), measured on the larger of the snippet and the real caller.
   Add the snippets to the context file; the developer-experience lens will
   correct them.
   *Done when* each developer has a snippet with a line count, or a sentence.

   **Stop only when the shape is wrong:** fixing it would discard most of the
   diff, or move the work to other files or another PR. Take that to the person
   with what you measured. A fix that adds to the diff is the first finding;
   continue.

4. **The cold read**, when the change has something a developer uses. Follow
   [`HUMAN-BAR.md`](HUMAN-BAR.md): record your own score now, before any lens
   runs; then, for
   someone else's change, show the person only the docs and the snippets and
   ask the three questions and one score. **Below 4, stop**: the change goes
   back with their words on what was unclear. For your own change, prepare the
   packet for your reviewer. With no person to ask, record your score, mark the
   cold read as pending, and continue.

5. **Run the lenses the change calls for**, with the briefs in
   [`LENSES.md`](LENSES.md): **standards** always; **spec and correctness**
   always, against the description when there is no spec; **developer
   experience** when a public surface, a sample, a user-facing screen, or an
   error a caller can receive changed. Each subagent has fresh context, reads
   the context file, and writes its report to a file you name. The file is the
   signal: a report is finished when its last line is `END OF REPORT`. A
   subagent's reply may never arrive, so look for the files.
   *Done when* each lens has reported findings, what it read, what it found
   clean, and what it declined to judge.

6. **Check the PR's own claims yourself, while the lenses run.** List every
   "tested", "verified", "passes" and "fixes" in the description, the commit
   message and the issue, each beside the test, script or CI job that shows it.
   A claim with nothing behind it is a finding (`BAR.md` §3), and it is posted,
   never left to the hand-over. "Fixes #N" is true only when the change does
   what that issue asks for; otherwise ask for "Part of #N". For each new
   control, name the test that fails if that line is removed; when you cannot
   run it, trace it and say it is a trace. When the author handed over proof
   ([`PROOF.md`](PROOF.md)), check that each piece is for this commit and
   matches the code; a claim whose proof is missing or does not match is a
   finding. For your own change, produce the proof now, by running it.

7. **Validate against the code.** First collect: besides each lens's findings,
   its public-surface rows marked "make internal" or "reshape", its "never
   asked for" list and its "has to know" list are candidate findings. Merge
   duplicates across lenses. Up to
   about eight findings, one fresh validator each; beyond that, group by area,
   five to ten to a validator. Validators also get your own findings from steps
   3 and 6, and each "clean" statement the review relies on. A finding survives
   only with evidence: the quoted line that shows it, or the read-only command
   that reproduces it.
   *Done when* every finding is confirmed, corrected, turned into a question,
   or dropped. Whatever a validator says was missed, check once yourself and
   label it, or send it as a question. An area no lens or validator covered
   gets one more pass. Then stop.

   Keep a **ledger**: every finding, and every point a validator confirmed or
   said was missed, gets exactly one destination: a comment, the body, Also
   noticed, pre-existing, or dropped with the reason, and its evidence level.
   Your own step 3 traces and each finding's remedy are rows too. Before step 9
   the counts add up.

8. **Write the review**, following [`WRITING.md`](WRITING.md) for labels, order,
   where each comment goes, and length. For someone else's change: a short body
   and comments on the changed lines, ending with the scorecard
   ([`SCORECARD.md`](SCORECARD.md)): six scores from the validated findings,
   and an overall score that is the lowest gate. Have one validator check the
   scores and the labels together before hand-over. For your own: fix what you found, then give
   your reviewer the cold-read packet, the scorecard, the proof, the findings,
   and what you did not verify: in the PR description in your own team's repository,
   privately in someone else's.

9. **Hand it over.** Show the person the review as it would be sent, the
   scorecard first, with
   both cold-read scores, what was not verified, what was declined, and the
   ledger's counts. Send or post only on their word. If they asked for files,
   write the review there. When the person tracks reviews in a project, propose
   the four tracking numbers ([`SCORECARD.md`](SCORECARD.md), "Tracking across
   pull requests") and write them on their word.
