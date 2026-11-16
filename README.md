# review-to-the-bar

A [Claude Code](https://claude.com/claude-code) skill that reviews a pull request before a human
reviewer spends time on it.

It starts where most reviews end: with the code a developer has to write to use the change. If a
sample app needs hundreds of lines to use one feature, that is the finding, and nothing further
down the diff matters as much.

## What it does

1. **Triage and pin** the pull request, and whatever calls it, to exact commits.
2. **Name the sources**: the repository's rules, the spec, and the callers.
3. **Measure the sample app first.** It writes the smallest correct code each kind of developer
   needs and counts the lines, beside what the real caller wrote. Over one screen, or needing
   explanation, is the first finding.
4. **Ask a person for a cold read.** Two or three minutes, three questions, one score from 1 to 5.
   Below 4, it stops there.
5. **Run the lenses the change calls for**: standards, spec and correctness, developer experience.
6. **Check the pull request's own claims** ("tested", "verified") against something that can be
   re-run.
7. **Validate every finding against the code**, and every "this is fine" the review relies on. A
   finding survives only with a quoted line or a command that reproduces it.
8. **Write the review**: blocking items first, each comment labelled, short enough to read. It
   ends with a scorecard: six scores from 1 to 5 (developer experience, shown is what is signed,
   correctness, evidence, scope, repository rules) and an overall score that is the lowest of
   the first three, never an average. What each score means is in
   [`SCORECARD.md`](skills/review-to-the-bar/SCORECARD.md).
9. **Hand it to a person.** Nothing is posted without their word.

It scales to the change: a small fix gets one reviewer and one validator, not a team.

It is read-only. It does not build or run a pull request's code unless you tell it to, and it
never approves or merges.

## Install

In Claude Code:

```
/plugin marketplace add openmobilehub/review-to-the-bar
/plugin install review-to-the-bar@review-to-the-bar
```

Choose the **user** scope so the skill is available in every repository you work in. From a shell:

```sh
claude plugin marketplace add openmobilehub/review-to-the-bar
claude plugin install review-to-the-bar@review-to-the-bar --scope user
```

To update later:

```sh
claude plugin marketplace update review-to-the-bar
claude plugin update review-to-the-bar@review-to-the-bar
```

Without plugins: copy `skills/review-to-the-bar/` to `~/.claude/skills/review-to-the-bar/`.

## Use

Ask Claude Code, in any checkout:

> Review https://github.com/owner/repo/pull/123 to the bar.

or call it by name with `/review-to-the-bar:review-to-the-bar`.

Two ways to use it:

- **Before you ask for review.** Run it on your own pull request, fix what it finds, and give your
  reviewer the cold-read packet, the scorecard, the findings, and what you did not verify (step 4
  below says where).
- **To check someone else's.** It produces a short review body, comments on the changed lines,
  and an appendix with every other true finding, one line each, for you to approve, edit or
  discard. A draft for someone else's repository carries no appendix.

## The process for one pull request

The skill is one step in a short process. The author does most of the work; the reviewer's part
is about ten minutes at the start and twenty at the end.

1. **Agree the design before any code.** The author writes a one-page design packet, usage
   first: the sample-app code a developer would type, with its line count, then the public
   signatures as a stub that compiles, and one alternative shape with why it lost. The reviewer
   checks that the usage fits on one screen, reads clearly, and has none of the red flags (such as
   several calls for one job). If it does not, the design changes now, while that is cheap.
   Nothing is built until the reviewer says yes. The packet and the red flags are in
   [`DESIGN.md`](skills/review-to-the-bar/DESIGN.md).

2. **Build.** The author builds the feature. A change that grows past the size limit is split
   into tickets first. On a board, the card stays in *In progress*.

3. **Self-review and proof.** The author runs `review-to-the-bar` on their own pull request and
   fixes what it finds. Then they prove each claim the change makes, by building and running it:
   a screenshot of the sample app as it ships, on every platform it draws on; the sample-app code,
   compiled and counted; each new check's test, failing with the check removed; a re-runnable
   script against the other side. Checking happens here, while a fix costs minutes, not after
   review. What cannot be proved is named, with the reason. The agent also answers the three
   cold-read questions below and records its own score. The full list is in
   [`PROOF.md`](skills/review-to-the-bar/PROOF.md).

4. **Hand it to your reviewer.** Send your reviewer the cold-read packet (the README section, the
   sample-app code with its line count, and the three questions), the scorecard, the proof, what
   the skill found, what you fixed, and what could not be proved.
   - **In your own team's repository:** put all of it at the top of the PR description.
   - **In someone else's repository:** send it privately, as a message or a comment on your
     team's board card. The PR description carries only what that repository's template asks
     for. Your scores are your team's working notes.

   Hand it over only when the scorecard is 4 or above on every gate (developer experience, shown
   is what is signed, correctness) and nothing is blocking. Only then does the card move to
   *In review*.

5. **The cold read.** The reviewer reads only the packet, not the diff and not the findings, and
   answers three questions:
   - **What is this for?** The use case, in one sentence.
   - **Is it simple?** A few lines of code.
   - **Where does it go?** Clear where the code belongs in an app.

   Then one score, 1 to 5, where 5 is "I could add this to an app now without asking anyone" and
   1 is "I cannot tell what this is for". **4 passes.** Below 4 the reviewer writes a line or two
   on what was unclear and the change goes back; they read nothing else. This takes two or three
   minutes, and it is the part no agent can do: an agent reads everything as clear.

6. **Acceptance check.** The reviewer compares their cold-read score with the agent's (a wide gap
   is itself worth a comment), checks the scorecard against the findings, skims the "not verified"
   list, and approves or sends back specific items. They can change any score. To spot-check, they
   run the skill themselves and get a draft review to edit or discard.
   If they track reviews in a private project, the skill proposes four numbers per pull request
   (rounds, score, cold-read gap, missed by self-review) and writes them on their word.

7. **Upstream, when the work lands in someone else's repository.** Until now the pull request was
   a draft, or lived in the team's fork. Once it passes the team's reviewer it goes to the
   upstream maintainers, who see work that has already cleared the team's bar.

8. **Merge.**

A pull request that reaches *In review* without the packet, the scorecard, the proof and the
findings goes back unopened. That rule is what makes the rest hold.

## The bar

The skill reads three layers of rules, from the base branch:

1. the target repository's own contribution, style and review files;
2. your project's rules when they live in another repository (tell it where);
3. the default in [`skills/review-to-the-bar/BAR.md`](skills/review-to-the-bar/BAR.md), five
   short rules that always apply.

To change what it holds a change to, change those files, not the skill.

## What is in the repository

| Path | What it is |
| :-- | :-- |
| `skills/review-to-the-bar/SKILL.md` | The nine steps. |
| `skills/review-to-the-bar/BAR.md` | The default bar. |
| `skills/review-to-the-bar/HUMAN-BAR.md` | The cold read: three questions and one score. |
| `skills/review-to-the-bar/LENSES.md` | The briefs for the reviewers and the validator. |
| `skills/review-to-the-bar/WRITING.md` | Labels, order, length, and where each comment goes. |
| `skills/review-to-the-bar/SCORECARD.md` | The six rubrics and how the overall score is set. |
| `skills/review-to-the-bar/DESIGN.md` | The one-page design packet agreed before code, and the red flags to screen for. |
| `skills/review-to-the-bar/PROOF.md` | What an author proves, by running their change, before asking for review. |
| `.claude-plugin/` | The plugin and marketplace manifests. |

## Where the ideas come from

Measuring the sample app first is this skill's own. The rest stands on other people's work:

- Reviewing standards and spec as separate lenses, so one cannot mask the other:
  [Matt Pocock's `code-review` skill](https://github.com/mattpocock/skills).
- Validating each finding against the code, and the list of things to leave out:
  [Anthropic's `code-review` plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/code-review)
  and [Claude Code Review](https://code.claude.com/docs/en/code-review).
- Saying what a reviewer declined to judge, and matching each claim to its evidence:
  [obra/superpowers](https://github.com/obra/superpowers).
- Design first, small changes, and tests that fail when the code breaks:
  [Google's Engineering Practices](https://google.github.io/eng-practices/review/).
- The comment labels: [Conventional Comments](https://conventionalcomments.org/).
- "Deliver code you have proven to work":
  [Simon Willison](https://simonwillison.net/2025/Dec/18/code-proven-to-work/).

## Contributing

Issues and pull requests are welcome. Sign off every commit (`git commit -s`), as in the other
[Open Mobile Hub](https://openmobilehub.org) repositories. A change to the steps or the briefs
should say what review it made better: the pull request it was tried on, and what it caught or
stopped flagging.

## Credits

The evidence ladder, the design packet and its red flags are adapted, in our own words, from
[pstack](https://github.com/cursor/plugins/tree/main/pstack) by Lauren Tan (MIT).

## License

[Apache-2.0](LICENSE).
