# AGENTS.md

Guidance for AI coding agents (Claude Code, Codex, Warp, etc.) working in this repository.

## What this repo is

A portable agent skill implemented entirely as Markdown. The runtime artifact is `SKILL.md`: the
agent reads its YAML frontmatter and the instructions below it. There is no build step, and the
repo should avoid wording that limits support to one or two harnesses.

## Key files

- `SKILL.md` — the skill itself. Portable YAML frontmatter (`name`, `description`, `license`,
  `metadata.version`) followed by the behavior specification: the fresh-vs-resumed session check,
  the mini quiz, the main conversation loop, the adaptive-level rule, the pronunciation
  constraint, and the progress-summary format. **This is the source of truth.**
- `README.md` — for humans: installation, usage, an example session, an overview of how the two
  phases (check-in, conversation loop) work, and a version history.

## The maintenance contract

`SKILL.md` and `README.md` must stay in sync. When you change behavior or content:

- **The pronunciation rule is load-bearing.** `SKILL.md`'s "The Pronunciation Rule" section exists
  specifically to stop the model from hallucinating an evaluation of the user's spoken accent when
  it only ever receives a text transcript. Do not soften, remove, or bury this section. If you
  touch it, keep it explicit and keep the README's reference to it (in the Usage section) pointing
  at the right heading.
- **Progress format:** the exact `INTERLOCUTOR PROGRESS SAVE` block format in `SKILL.md` (under
  "Ending a Session and Saving Progress") and the example in `README.md` (under "Resuming a
  session") must match field-for-field. If you add, rename, or remove a field, update both.
- **Version:** `SKILL.md` frontmatter stores the version under `metadata.version`; `README.md` has
  a "Version History" section. Bump them together. Keep the version under `metadata`; a top-level
  `version` key is not portable across Agent Skills hosts.
- **Compatibility:** keep install and usage language harness-neutral. The skill should work in any
  agent harness that can load Markdown skill instructions, and in Claude's voice mode
  specifically, since transcription is central to the pronunciation constraint above. Claude Code
  and other harnesses are examples, not limits.
- **Validation:** before publishing, sanity-check the frontmatter is valid YAML and that
  `npx skills add . --list` recognizes the skill.
- **Non-obvious fixes:** if you change the prompt to handle a tricky failure mode (the model
  drifting into English mid-conversation, skipping the alternative-phrasings step, or announcing a
  difficulty change out loud), add a short note to the README version history explaining what was
  fixed and why.

## Editing SKILL.md

- Preserve valid YAML frontmatter (formatting and indentation).
- The instructions below the frontmatter are the product. Edit them like a careful specification
  for how a conversation should behave, not like code.
