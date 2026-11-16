# The design packet, before any code

Reference for step 1 of the process in the README, and for the
developer-experience lens in [`LENSES.md`](LENSES.md). Before building a
feature, the author agrees its shape with the reviewer on one page. Changing a
design costs minutes here and days after the code exists.

## The packet

One page, in this order. Write the usage first: the shape follows from it.

1. **The problem.** One paragraph: what the feature is for, and the constraints
   that make its shape less than obvious (callers that cannot break, a spec it
   must follow, types it must work with).
2. **The usage.** What a developer reads (the README or quickstart section) and
   the sample-app code they write, for each kind of developer, with its line
   count. This is the measurement in `BAR.md` §1, made before the code instead
   of after.
3. **The shape.** The public signatures, as a stub that compiles against the
   usage: types, functions, what they return, how they fail. Say what it hides
   from the caller, and what it deliberately does not do.
4. **One alternative, and why it lost.** At least one other real shape, judged
   by what it would make the caller write and know. If the constraints left only
   one shape, say why.
5. **Tradeoffs.** One line each: "we accept this in exchange for that".
6. **Questions for the reviewer.** What the author wants decided, as questions.

The reviewer checks the usage first: one screen, readable, in one place, and
the steps that must happen together are one call. Nothing is built until they
agree. The agreed packet goes with the PR, and the review measures the change
against it.

## Red flags

Screen the shape for these. Each is a reason to change it before code exists,
and a finding when it shows up in a PR.

- **Several calls for one job.** The caller has to coordinate more than one
  call, or several places, to do one thing, and leaving one out is not refused.
  Make it one call, or have the library refuse when a part is missing.
- **Options that expose the inside.** Parameters that select internal stages or
  implementation choices the caller should not have to understand.
- **Two ways to do one thing.** Callers copy whichever they find first, so both
  keep spreading. Keep one.
- **Internals a caller can reach.** If an internal type or function can be
  imported, it becomes part of the API. Make it unreachable from outside.
- **Wire formats in the public API.** Protocol or storage types handed to the
  caller. Parse them into the library's own types behind the interface.
- **The same rule in two places.** One decision, such as a format or a
  validation, written in more than one module, so changing it needs edits in
  each. Give it one owner.
- **Lists kept in step by hand.** The same items listed in two places, so
  adding one means editing both. Derive one from the other, or make the build
  fail when they disagree.
- **Layers that only forward.** A function that passes its arguments to another
  of the same shape and adds nothing. Remove it.
