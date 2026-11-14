# The default bar

The minimum every review holds a change to. A project's own rules add to it;
where a project rule is stricter, the project rule wins.

## 1. The sample app is the measurement

The code a sample app needs in order to use a feature is the code every
developer will have to write, so its length and readability are properties of
the API.

- It fits on one screen: about 30 lines for one feature.
- It reads top to bottom as what the developer is trying to do: no setup block
  before the point, no type they must look up to follow it, names that state
  what they do. Someone new to the library follows it without opening the source.
- Longer than that, or it needs explaining: the API is not finished. The usual
  fix is to move the work into the library, with an override for an app that
  wants its own.
- Things that must be done together are one call, or the library refuses when
  half is missing.

## 2. What is shown is what is signed and enforced

Where a person approves something that is then signed or enforced:

- every term the approval covers is visible before it is given;
- each value the other side enforces (an id, an amount, a date, a key) is shown,
  and not only its friendly name;
- a value that cannot be parsed or displayed is a refusal, never an omission.

## 3. Claims can be re-run

"Tested", "verified" and "passes" each point at a test, a script or a CI job
that anyone can run again. Where a spec and its reference implementation
disagree, the change says which one it follows and why.

## 4. The API cannot be used wrongly by accident

- Public names say what the thing does; no two different things share a name.
- Types cannot hold a state that fails later.
- A refusal tells the developer why, in an error they can read.
- Data meant for display is structured (kinds, numbers, dates), so an app can
  format and localise it.

## 5. One purpose, and small enough to read

A change does one thing. Past about 400 changed lines of source it is one
ticket of a written plan, and unrelated changes go in their own change.
