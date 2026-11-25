# Agent Instructions

Primary entrypoint for any agent (Claude, Codex, DeepSeek, etc.) working in this repository.

Keep this file under ~200 lines — it is loaded into context every session. Details belong
in `AGENTS/` docs; repeatable procedures belong in skills under `.claude/skills/`.

## Project

<name> — <one-line purpose>

## Environment

- OS / shell: see `AGENTS/ENV.md`
- Commit identity: `wildcar <wildcar@mail.ru>`
- Details, credentials, command cheat-sheet: `AGENTS/ENV.md`

## Document Map

| File | Role |
|------|------|
| `AGENTS.md` | This entrypoint. Workflow, rules, map. |
| `CLAUDE.md` | Compatibility pointer to `AGENTS.md`. |
| `AGENTS/SPEC.md` | Functional specification — source of truth for product behavior. |
| `AGENTS/STATE.md` | Current snapshot: goal, now, next, open questions, deferred. Overwritten each iteration. |
| `AGENTS/HISTORY.md` | Append-only iteration log, newest first. Read only the top few entries. |
| `AGENTS/MEMORY.md` | Durable cross-session memory: working agreements + project facts. The ONLY agent memory store — see Memory. |
| `AGENTS/ENV.md` | Host, tools, credentials, command cheat-sheet. Read on demand. |
| `README.md` | Public-facing readme: what the project is, how to run it locally. |
| `docs/` | Domain / reference docs. Read on demand when a task touches that area. |
| `docs/adr/` | Architecture Decision Records — one file per significant decision (see `docs/adr/TEMPLATE.md`). |

## Startup Checklist

1. Read `AGENTS.md` (this file).
2. Read `AGENTS/SPEC.md`.
3. Read `AGENTS/STATE.md`.
4. Read top 3–5 entries in `AGENTS/HISTORY.md`.
5. Read `AGENTS/MEMORY.md` (working agreements + durable facts).
6. Check `git status --short` before editing; do not overwrite unrelated user changes.

Open `AGENTS/ENV.md` only when you need environment details. Open the relevant file under `docs/` when the task touches that domain.

## Change Workflow

For every iteration that changes code or behavior:

1. If the functional contract changes — update `AGENTS/SPEC.md` first.
2. Make the changes.
3. Overwrite `AGENTS/STATE.md` to reflect the new current state.
4. Prepend a new entry to `AGENTS/HISTORY.md` using the format below.
5. Commit and push once the Definition of Done (below) is met.

### `AGENTS/HISTORY.md` entry format (≤5 lines, newest first)

```
## YYYY-MM-DD · <short iteration title>
- What: <one line — what changed>
- Why: <one line — reason / task>
- Files: <key paths, comma-separated>
- Next: <one line — what was planned right after>
```

Keep each entry tight. Long explanations belong in commit messages or `SPEC.md`. An optional `Fixes-on-the-fly:` line is fine when an iteration also corrected small things discovered mid-way.

When `HISTORY.md` grows past ~200 lines, move the oldest entries to `AGENTS/HISTORY-ARCHIVE.md` (create it on demand) and keep the recent ones here.

When you ship a deferred item from `STATE.md`, write a normal HISTORY entry and remove the item from `STATE.md`.

Parallel work: only the top-level session writes `STATE.md` and `HISTORY.md`. Subagents and secondary sessions report back instead of editing these files.

## Definition of Done

An iteration is done only when:

1. Build succeeds.
2. Lint / typecheck is clean.
3. Tests pass.
4. SPEC / STATE / HISTORY (and MEMORY, if a durable fact emerged) are updated.

Commands for 1–3 live in Stack & Commands. A step that does not apply yet (e.g. the project has no test suite) goes to `AGENTS/STATE.md` → Deferred instead of being silently skipped.

## Memory

`AGENTS/MEMORY.md` is the **single** store of durable agent memory in this project.
Do not use external or per-tool memory stores (memory directories outside the repo, a
tool's built-in memory, etc.): memory must travel with the repository when cloned.

- Read `AGENTS/MEMORY.md` at the start of every session (see Startup Checklist).
- When you learn a durable fact or a working agreement, append a short bullet there and
  commit it together with the related change.
- Split of concerns: durable facts/agreements -> `MEMORY.md`; current snapshot ->
  `STATE.md`; iteration log -> `HISTORY.md`.

Recording rules — keep these a habit:

- One bullet = one fact; keep it short. Long explanations belong in commit messages or `SPEC.md`.
- For working agreements, add a brief **why** so the rule doesn't look arbitrary.
- Convert relative dates to absolute ("today" → the concrete date).
- Do NOT record what is already in the code, git history, or SPEC/STATE/HISTORY.
- Consolidate from time to time: merge duplicates, drop stale or wrong entries.

## Language Rules

- Source code, technical docs, code comments: English.
- Conversation with the user: Russian.
- End-user UI text: Russian, with ability to extend to other languages.
- Russian prose for humans (replies to the user, UI copy, user-facing docs, reports, release notes) follows Communication & Writing Style below.
- Existing docs already written in another language are an established contract — keep editing them in that language; don't silently translate.

## Communication & Writing Style

Keep answers short and to the point, no filler. We're having a dialogue: I ask or say something, you respond. First answer the question directly; optionally add one alternative or a follow-up question to continue the conversation. If you use specialized technical terms or jargon, add a brief explanation in parentheses. Prefer a thoughtful, general, correct solution over quick fixes, hacks, or hardcoding.

Write like a human being. Keep the meaning, facts, numbers, and examples; cut filler and stock phrases.

Banned (find one in your text and rewrite it):
- Negative parallelisms: "not just X, but Y", "not only X but also Y", "this isn't just...".
- The rule of three: three similar words in a row to look complete ("quality, reliability, and efficiency").
- Announcements and transitions: "it's important to note", "it's worth emphasizing", "let's break this down", "here's what you need to know".
- Summarizing endings: "in summary", "in conclusion", "thus" at the end. An ending adds something new, or the text just stops.
- Chatbot artifacts: "great question", "you're absolutely right", "hope this helps", "let me know if...".
- Fake intimacy and pseudo-depth: "honestly?", "let me be direct", "here's the thing", "at bottom", "the real question is".
- Aphorism formulas: "X is the language of Y", "X is the currency of Z", "X is the new Y".
- Choppy drama: stacks of short fragments in a row ("Without X. Without Y. Only Z."). One short sentence for emphasis is fine; three in a row is not.
- Rhetorical questions.
- Trailing participles that fake depth: "...highlighting", "...reflecting", "...contributing to".
- AI vocabulary: key, crucial, pivotal, marks, symbolizes, underscores, showcases, fosters, enables, shapes, vibrant, rich (figurative), unique, stunning.
- Bureaucratese: "is", "represents", "serves as", "carry out", "provide assistance", "conduct work". Use "this is" or a plain verb instead (help, work).
- Stop words and intensifiers: "in today's world", "nowadays", "as we know", "very", "truly", "really", "certainly". If deleting the word leaves the meaning unchanged, it's filler.
- Vague attributions: "according to experts", "analysts note". Either name a specific source or state it yourself.
- Symbols instead of words: = → > < + vs & ≈. Write it out in words.
- Jargon without explanation: iteration, validation, optimize, case, flow, functionality. Write for a smart 12-year-old.
- Emoji, bold on every word, lists like "Heading: restating the heading".
- Guesses instead of data: if you don't know, say you don't know.

How to write:
- A verb instead of a verbal noun, active instead of passive.
- Replace judgment with fact: not "fast" but "responds in a second".
- Alternate short and long sentences. One sentence, one thought; one paragraph, one topic; the main idea first.
- Have an opinion, admit mixed feelings, use "I" where it fits.
- Don't touch quotes, names, titles, or specific odd details.
- Answer with the single most applicable option; don't pile up several. You can note that an alternative exists in one short phrase.
- Before you hand it over, reread it and ask yourself: "What here still gives away the model?" Rewrite whatever you find.

## Commands for the User

Any command handed to the user to run (bash, PowerShell, cmd, a script invocation, etc.) must, where possible, work from any current directory and under any user account:

- No reliance on the current directory: use absolute paths, or make the command locate its own base (`git -C <repo>`, `cd "$(dirname "$0")"` in bash, `$PSScriptRoot` / `Set-Location` in PowerShell).
- No hardcoded user-specific paths or names (`C:\Users\<name>`, `/home/<name>`): use `$HOME` / `~`, `$env:USERPROFILE`, `%APPDATA%`, `$env:LOCALAPPDATA`, etc.
- If the command needs specific rights (admin / `sudo`) or a specific account, say so explicitly next to it.
- If location- or user-independence is impossible, state the precondition right before the command (e.g. "run from the repository root").

## Project Rules

Hard constraints and invariants this project must not violate. Fill in after the first iteration; keep each rule one line.

<!-- Examples of the kind of rule that belongs here:
- Do not delete files — move retired code to `deprecated/` (or `git mv`) so git history survives.
- Library / framework lock-ins ("X is the only allowed library for Y").
- Runtime / boundary invariants the agent must respect.
-->

- —

## Stack & Commands

Stack one-liner plus the commands an agent needs on day one. Keep the full cheat-sheet in `AGENTS/ENV.md`; here keep only the essentials.

```bash
# install      — <how to install dependencies>
# dev / run    — <how to start the app locally>
# build        — <how to produce a production build>
# test         — <how to run the test suite>
# lint         — <how to lint / typecheck>
```

## Architecture

Map of the codebase so an agent knows where things live. Fill in after the structure stabilizes.

```
<top-level layout — key directories and their responsibility>
```

## Code Style

- <language mode / formatter / linter conventions>
- <naming, units, formatting rules that aren't obvious from the code>
- Match the conventions of surrounding code: comment density, naming, idiom.
