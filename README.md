# vibecheck-lean

A Claude Code, Codex and OpenCode skill that takes someone with no formal-methods experience to
proving properties of real software in Lean 4, built for the
[VibeCheck](https://fmxai.org/vibecheck/) hackathon.

Load the skill in your agent and say anything Lean-related ("I want to learn Lean",
"help me verify my repo"). The skill first asks which rung you're on:

| Rung | What you get |
| --- | --- |
| I know Lean; spec my project | A 15-minute spec interview grounded in your code, a catalogue of spec patterns, then the route that fits your language: port, prove and differential-test; Rust extraction via hax/Aeneas; Veil for protocols; or Lean-native |
| Build an app in Lean | Three starter apps with specs written and proofs left to you (undo/redo editor, CRDT counter, run-length codec), app recipes (CLI, JSON, files, a verified key-value REPL), and a project layout with a human-owned spec |
| Learn Lean | Grow a runnable API checker through eight levels, from `#eval` to proofs about policies, retry plans and a compiler; standalone exercises and language bridges remain available |
| Keep me sharp | Short refreshers: a freshly written, compiler-checked drill on whatever is due by spaced repetition, plus one new idea per session |

Learn in chat by pasting Lean code into the conversation. The assistant runs it locally and returns compiler output and goals.
Successful declarations carry into later snippets; each submission and result stays in the project's `.vibecheck/chat/` folder.
Missing Lean and build dependencies are installed as a setup step, with authorization, before starting the lesson.

For practical learning, grow one API checker through the eight levels: command execution, response policies, retries, budgets and proofs.
The starter runs curl immediately. Later lessons prove properties of its pure core; network I/O remains outside those proofs.
Refreshers revisit the same program. Compact command output keeps debug records on disk, with occasional Lean humor in the conversation.

You can also use the editor: VS Code on the right file (the Lean 4
extension installed first, with your OK), a start page for your rung (next
steps, a Loogle search box, tactic cheat sheet, what's due), and Loogle. Your
progress lives in `~/.vibecheck/profile.json`, so it follows you between projects.

On every rung, the audit checks a project the way a judge would: it builds, has
no `sorry` or other shortcuts, the spec is unchanged since you approved it, and
every property is proved on Lean's standard axioms. It writes `VERIFICATION.md`.

## Install

On macOS, Linux or WSL2 with git installed, this command installs the pinned event preview:

```bash
curl -fsSL https://raw.githubusercontent.com/modiqo/vibecheck-lean/v0.1.0-event-preview/install.sh | bash
```

The installer places one copy under `~/.agents/skills/` and links Claude Code to it.
It preserves existing installations. Start a new agent session and ask to learn Lean.
For a review before execution, download [install.sh](install.sh), read it, then run it with Bash.

For a manual installation, clone the preview into the shared location:

```bash
mkdir -p "$HOME/.agents/skills"
git clone --depth 1 --branch v0.1.0-event-preview https://github.com/modiqo/vibecheck-lean.git "$HOME/.agents/skills/vibecheck-lean"
```

Copy this skill folder, including its scripts and assets, into your harness's skill directory:

| Harness | User-level location |
| --- | --- |
| Claude Code | `~/.claude/skills/vibecheck-lean/` |
| Codex | `~/.agents/skills/vibecheck-lean/` |
| OpenCode | `~/.config/opencode/skills/vibecheck-lean/` |

For one shared installation, keep the folder at `~/.agents/skills/vibecheck-lean/`.
Codex and OpenCode discover that location. Add a Claude Code link to the same folder:

```bash
mkdir -p "$HOME/.claude/skills"
ln -s ../../.agents/skills/vibecheck-lean "$HOME/.claude/skills/vibecheck-lean"
```

Run the link command only when the destination does not already exist. Use `.agents`, with the final `s`.

See [harness integration](references/harnesses.md) for project locations, official documentation and optional MCP setup.
The same CLI and profile work across the three hosts. Tool activity rows and timing badges follow each host's display controls.

Then load the skill and ask one of the questions above. Requirements:
macOS or Linux (Windows through WSL2), Python 3.10+, git. Everything else the
skill installs for you.

## Event preview and checks

The chat runner, local HTTP fixtures and relocated installation have automated tests on macOS with Lean 4.35.0.
Interactive Claude Code and OpenCode flows, and fresh Linux dependency installation, have not been tested end to end.
Install dependencies before the event. The project track supplies a working seed and guided milestones; later features are built during lessons.

From this skill folder, these commands print a dependency plan and run the workflow tests:

```bash
python3 scripts/setup.py --rung learn --mode chat
python3 -m unittest discover -s tests -v
```

After approving any missing dependencies, ask your agent to apply the plan and start the lesson.
The proof audit reports an unlocked spec; review and lock the intended properties before relying on spec-change checks.

## License status

This event preview is published for review with the license decision pending. It does not yet include an open-source LICENSE file.

## Local data

The runner executes submitted Lean with the host's normal permissions. It is not a sandbox.
Profiles, submitted code and compiler output stay in local profile and project folders.
Dependency installers download their tools; API examples contact the endpoint you select.
Generated projects ignore chat transcripts, generated start pages, progress notes and environment files in Git.
They include portable audit tools, so sharing a project does not require publishing the skill's local installation path.

## Before the event

Do the set-up at home: the Lean toolchain is a few hundred MB, and the Rust
route (hax plus Aeneas plus Mathlib) needs about 8 GB and a few minutes of
downloads that venue Wi-Fi will not enjoy.

## Contents

```
SKILL.md                      entry point: placement question, rungs, rules
scripts/profile.py            learner profile and spaced-repetition schedule
scripts/launch.py             opens VS Code, the start page and Loogle for a rung
scripts/doctor.py             read-only environment check
scripts/setup.py              plan, install and recheck missing requirements for the chosen mode
scripts/bootstrap.sh          install steps: prereqs, elan, vscode, uv, rust, hax, node, mcp, open
scripts/chat.py               spool chat submissions, run Lean and return compiler results
scripts/new_project.py        project scaffold, optionally from a starter
scripts/detect_repo.py        language detection, route choice, first targets
scripts/audit.py              proof audit and VERIFICATION.md
assets/exercises/             the eight-level ladder, plus solutions/
assets/project-ladder/        runnable API checker seed for the incremental project track
assets/bridge/Bridge.lean     Python/TypeScript idioms in Lean
assets/starters/              undo, gcounter, rle (with reference solutions)
assets/recipes/               KvApp.lean (complete verified app), IoRecipes.lean
assets/spec-patterns/         one compiled example per spec-interview question
assets/template/, difftest/   scaffold files; differential-test server and harness
references/                   primer, learn, refresh, spec-elicitation, app-recipes,
                              track playbooks, judging, other tools
```

Tested on 2026-10-30 with Lean 4.35.0 and cargo-hax 0.4.2.
