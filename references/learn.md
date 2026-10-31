# Learn mode: from zero to comfortable

The goal is a person who can state a property of their own code and prove it
without you. Proofs you write for them do not get them there; proofs they write
with you watching do.

## Getting them set up to learn

Default to `references/project-ladder.md` as the main progression, or adapt its milestones to the learner's own useful program.
Grow the API checker through L1–L8 and keep this ladder's learning goals and grading rules.
Use standalone exercises as optional drills rather than replacing the learner's project at each level.
If the learner requests standalone exercises, save `learning_track` as `exercises` and follow the setup below.

1. **A playground project.** Learners who don't have a project yet get one:
   `new_project.py playground --dir ~/vibecheck` (ask before choosing the
   location). Exercises are copied into its `Learn/` folder, which the audit
   ignores. Record it: `profile.py set project ~/vibecheck/playground`.
2. **Placement.** Experience "none": start at level 1. "Tried it a little" or
   "comfortable": don't take their word or make them repeat basics. Probe levels
   2, 4 and 6 in that order, one exercise each, about five minutes apiece, and stop
   at the first one they don't solve cleanly (needed a hint, or couldn't). Start
   there: a probe they struggled on, or the level below a probe they couldn't do.
   Example: L2 clean, L4 needed a hint → start at L4 (skip probing L6). Record each
   probe with `profile.py review L<n> --result …`; levels below the start that
   weren't probed need no record.
3. **Their language.** If they know Python, TypeScript or JavaScript, the bridge
   (`<skill>/assets/bridge/Bridge.lean`: every Python/TS idiom with its Lean
   equivalent, all compiled, with `#eval` output to read) opens next to the
   exercise; `launch.py` copies it into `Learn/` for the learn rung when their
   profile lists one of those languages. For Rust or other languages, point out
   the closest idioms as they come up.
4. **In chat by default.** Read `references/chat-workspace.md`. Use
   `launch.py learn --project <playground> --level L<n> --mode chat` to copy the exercise.
   Show the worked example here, then execute the learner's submissions with `chat.py`.
   No editor, extension or browser is required.
5. **Editor when preferred.** `launch.py learn --project <playground> --level L<n> --mode editor` copies the
   exercise, opens VS Code on it (installing the Lean extension first if they
   agreed), and opens the start page and Loogle in the browser.

## How to teach

- **Show, then share, then hand over.** For each new idea: you do a worked example
  and narrate every goal change; then you do the next one together (they choose
  the tactic, you explain the result); then they do one alone while you only
  answer questions.
- **Always point at the goal.** After every tactic, quote the new goal and say in
  one sentence what changed. This is the habit that makes people self-sufficient.
- **Hints in three steps.** Asked for help on an exercise: first a question ("what
  does the goal look like after `unfold`?"), then the tactic name, then the line.
  Never the full solution unless they ask for it outright.
- **Connect every exercise to software.** "This is the rate limiter's budget
  check", "this is how you prove a fast path equals the slow path".
- **Keep sessions short.** One level per sitting is plenty. End each with what
  they can now do that they could not before.

## The ladder

Exercise files live in `assets/exercises/` (solutions in `assets/exercises/solutions/`;
never open those in front of the learner unless asked). Copy a level into the
learner's project folder as `Learn/LN_Name.lean`, open it in the editor, and work
through it. A level is done when `lake env lean Learn/LN_Name.lean`, run in the
project folder (or the file in the editor), shows no errors and no `sorry` warnings.

| Level | File | New idea | The software skill it unlocks |
| --- | --- | --- | --- |
| 1 | `L1_Programs.lean` | `def`, `#eval`, `rfl`, `decide` | Lean as a language; facts by computation |
| 2 | `L2_Unfold.lean` | `unfold`, `omega`, statements about all inputs | Arithmetic safety: no overflow, no negative prices |
| 3 | `L3_Branches.lean` | `split`, `grind`, hypotheses from `if` | Guards and validation: rate limiters, permission checks |
| 4 | `L4_Induction.lean` | `induction`, recursive functions | Anything over lists, trees, logs |
| 5 | `L5_Reference.lean` | Generalized lemmas, fast-equals-reference | Optimizations that provably change nothing |
| 6 | `L6_Loops.lean` | `mvcgen`, loop invariants | Imperative code with `let mut` and `for` |
| 7 | `L7_Invariants.lean` | Structures carrying proofs | Illegal states unrepresentable: bounded queues, counters |
| 8 | `L8_Compiler.lean` | Induction over syntax trees | Interpreters, compilers, query planners |

After level 3 a learner can contribute to a hackathon project. After level 5 they
can verify most pure business logic. Level 8 is the "I proved a compiler correct"
moment; celebrate it.

## Teams with a deadline

A hackathon team usually wants a result today, not a course. Two ways to fit
learning in:

- **Teach inline.** Skip the ladder and narrate the real project's proofs as you
  write them, using the habits above. Offer the ladder afterwards.
- **Compressed warm-up.** Levels 1–3 together in about 30 minutes, one person
  driving, then straight into the project. Level 3 exercise 3 is a tiny rate
  limiter, a good bridge into real code.

Exercise files in `Learn/` are ignored by `audit.py`, so unfinished ones never
block a submission.

## Recording progress

The learner's profile (`scripts/profile.py`, stored in `~/.vibecheck/profile.json`)
follows them across projects. After each level:

```bash
python3 <skill>/scripts/profile.py review L3 --result pass        # or struggled / fail
python3 <skill>/scripts/profile.py weak add "induction generalizing"   # something that didn't click
python3 <skill>/scripts/profile.py log learn "L3 done; split clicked, grind felt like magic"
```

`review` also schedules the level for spaced review, so a learner who later moves
to the refresh rung gets drills on exactly what they learned and struggled with.

## Beyond the ladder

Point learners who want more at, in this order:

- [Functional Programming in Lean](https://lean-lang.org/functional_programming_in_lean/)
- [Theorem Proving in Lean 4](https://lean-lang.org/theorem_proving_in_lean4/)
- The [mvcgen tutorial](https://lean-lang.org/doc/tutorials/4.34.0/mvcgen/)
- [Lean Zulip](https://leanprover.zulipchat.com), stream `#new members`
- [Loogle](https://loogle.lean-lang.org) to search lemmas by shape

The real graduation is their own code: pick a function from something they
maintain, write `Spec.lean` for it, and prove it.
