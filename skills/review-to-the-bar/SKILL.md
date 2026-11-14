---
name: review-to-the-bar
description: Use to review a pull request before a human reviewer spends time on it — your own before you ask for review, or someone else's you were asked to check — in any repository, especially one where no review bot runs. Measures the sample app first, asks a person for a two-minute cold read, runs the lenses the diff calls for (standards, spec and correctness, developer experience), validates every finding against the code, and ends in a review a person approves before anything is posted.
---

# Review to the bar

A review that finds what a careful human reviewer would find, before that
reviewer has to. Read-only throughout: read the code, and run read-only `git`
and `gh` (`gh pr checks` included). A pull request's checkout is someone else's
code, so build or run it only when the person you are working for says to, and
say in the review what you did not run. A person decides what is posted and
whether the change merges; this skill never approves or merges.

## Which review this is

Settle these two before step 1; they change what you produce.

- **Whose change.** *Your own* when the PR's author is the account you run as,
  or the person says so. Otherwise *someone else's*. A teammate's PR in another
  project's repository is someone else's, and its review goes to the author
  privately unless the person says to post it.
- **How big.** Count changed lines of source, leaving out tests, docs, lockfiles
  and generated files.
  - **Small**, under about 100: one reviewer runs every brief that applies, in
    turn, and one validator takes all the findings.
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
   elsewhere.** Ask the person, or read their instructions, for where it is. In
   someone else's repository, a rule that only your project's bar holds is
   reported to your own team; posted there, it is a suggestion at most.
3. **[`BAR.md`](BAR.md)**, the default: the minimum that always applies.

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
     same author on the same issue (diff and description only). Pin each
     caller to a commit as well; a caller that moves under the review hides
     what it really had to write.

   Write these, the pin, the diff command, the PR title, description and commit
   message, and every limit the person set, into **one context file**. Every
   subagent reads it.

3. **Measure the sample app.** Read only what a caller sees: public signatures,
   docs, the README, an existing sample. For each kind of developer the change
   touches, write the smallest correct code they need today and count the lines
   as a sample app would format them; for a developer who writes nothing new,
   one sentence saying so. Beside each minimal snippet, give the number of
   lines the real caller wrote at its pinned commit; when the two differ, the
   larger is what developers will copy. For a fix, measure before and after.
   Say the snippets are uncompiled when you could not build. Over one screen, or needing
   explanation, is the first finding (`BAR.md` §1). Add the snippets to the
   context file; the developer-experience lens will correct them.
   *Done when* each developer has a snippet with a line count, or a sentence.

   **Stop only when the shape is wrong:** fixing it would discard most of the
   diff, or move the work to other files or another PR. Take that to the person
   with what you measured. A fix that adds to the diff is the first finding;
   continue.

4. **The cold read**, when the change has something a developer uses. Follow
   [`HUMAN-BAR.md`](HUMAN-BAR.md): record your own score first; then, for
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
   the context file, writes its report to a file you name, and replies with the
   path.
   *Done when* each lens has reported findings, what it read, what it found
   clean, and what it declined to judge.

6. **Check the PR's own claims yourself, while the lenses run.** List every
   "tested", "verified", "passes" and "fixes" in the description, the commit
   message and the issue, each beside the test, script or CI job that shows it.
   A claim with nothing behind it is a finding (`BAR.md` §3). For each new
   control, name the test that fails if that line is removed; when you cannot
   run it, trace it and say it is a trace.

7. **Validate against the code.** Merge duplicates across lenses first. Up to
   about eight findings, one fresh validator each; beyond that, group by area,
   five to ten to a validator. Validators also get your own findings from steps
   3 and 6, and each "clean" statement the review relies on. A finding survives
   only with evidence: the quoted line that shows it, or the read-only command
   that reproduces it.
   *Done when* every finding is confirmed, corrected, turned into a question,
   or dropped. Whatever a validator says was missed, check once yourself and
   label it, or send it as a question. An area no lens or validator covered
   gets one more pass. Then stop.

8. **Write the review**, following [`WRITING.md`](WRITING.md) for labels, order,
   where each comment goes, and length. For someone else's change: a short body
   and comments on the changed lines. For your own: fix what you found, then put
   the cold-read packet, the findings, and what you did not verify at the top
   of the PR description.

9. **Hand it over.** Show the person the review as it would be posted, with
   both cold-read scores, what was not verified, and what was declined. Post
   only on their word. If they asked for files, write the review there.
