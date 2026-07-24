# Interlocutor

[![skills.sh installs](https://skills.sh/b/vini-porto/interlocutor)](https://skills.sh/vini-porto/interlocutor)

A portable agent skill that turns Claude into a foreign-language conversation partner, in text or
voice mode. It is plain Markdown, so it can run in any harness that supports skill-style
instructions.

## Installation

### Skills CLI

Install globally with the cross-agent skills CLI so Interlocutor is available in every project:

```bash
npx skills add vini-porto/interlocutor --global
```

Update an existing install:

```bash
npx skills update interlocutor --global
```

To install globally into every supported agent harness:

```bash
npx skills add vini-porto/interlocutor --global --agent '*'
```

To target one configured harness, pass its agent name:

```bash
npx skills add vini-porto/interlocutor --global --agent <agent-name>
```

Omit `--global` for a project-local install that can be committed and shared with collaborators.
Start a new agent session or reload skills after installation.

### Manual

Any agent harness can use the skill directly because the runtime artifact is `SKILL.md`. Install
it wherever your harness expects skill directories, or copy `SKILL.md` into an existing skill
folder.

For example:

```bash
git clone https://github.com/vini-porto/interlocutor.git /path/to/your/skills/interlocutor
```

Or, if you already have this repo cloned:

```bash
mkdir -p /path/to/your/skills/interlocutor
cp SKILL.md /path/to/your/skills/interlocutor/
```

For Claude Code specifically, that usually means a directory under `~/.claude/skills/` (global)
or `.claude/skills/` (project-local):

```bash
mkdir -p ~/.claude/skills/interlocutor
cp SKILL.md ~/.claude/skills/interlocutor/
```

## Usage

Open a new chat and invoke the skill however your harness exposes installed skills, then just
start talking. You don't need to fill out anything up front, Claude will ask.

```
Let's practice a language.
```

You can also front-load what you already know to skip straight past the parts of the check-in
you'd rather not repeat:

```
I want to practice Portuguese conversation. I'm somewhere around B1, and I like
talking about travel and music.
```

Works the same way in Claude's voice mode: open a voice chat with the skill installed and just
start speaking. Since voice input reaches Claude as a text transcript, feedback stays limited to
grammar, vocabulary, and phrasing. Interlocutor never invents an opinion about your accent or
pronunciation; see [The Pronunciation Rule](SKILL.md#the-pronunciation-rule) in `SKILL.md`.

### Resuming a session

At the end of a session, Claude prints a short plain-text progress summary. Save it somewhere (a
note, a file, whatever's convenient), and paste it back in as your first message next time:

```
INTERLOCUTOR PROGRESS SAVE
Language: Portuguese
Level: B1, upper end
Topics covered: travel, weekend plans, ordering food
Recurring errors: subjunctive after "quando", gender agreement on adjectives
Strengths: good vocabulary range, comfortable with past tense narration
Notes: prefers informal register, enjoys travel and music topics
```

Claude reads it, skips the check-in, and picks the conversation back up from there. If you're
running this in an environment with real file access (like a local Claude Code project),
Interlocutor can also read and write a local `progress.md` as a fallback, but this is optional:
the copy-paste summary is the only mechanism guaranteed to work everywhere, including claude.ai
with no disk access.

## Example Session

**First message (fresh session):**

> **User:** hey, want to practice a language with you
>
> **Claude:** Sure, let's do it! Which language do you want to practice, and roughly where are
> you at with it, total beginner, can hold a basic conversation, pretty comfortable but rusty,
> whatever fits? And what do you actually like talking about, travel, work, movies, news,
> anything?
>
> **User:** Spanish. I'd say... intermediate? I took it in high school and can get by. I like
> movies and food.

**Claude switches fully into Spanish and opens the loop:**

> **Claude:** *¡Genial! Empecemos con algo sencillo. ¿Cuál fue la última película que viste, y qué
> te pareció?*

**User replies with an error:**

> **User:** *Yo vi una película muy buena la semana pasado. Se llama "Coco" y me hizo llorar.*

**Claude corrects, then offers alternatives, then continues:**

> **Claude:** *¡"Coco" es preciosa, totalmente de acuerdo! Un pequeño detalle: dijiste "la semana
> pasado", pero "semana" es femenino, así que es "la semana pasad**a**". Todo lo demás estuvo muy
> bien.*
>
> *Otras formas de decir lo mismo:*
> - *Más formal: "Vi una película excelente la semana pasada."*
> - *Más informal: "Vi una peli buenísima la semana pasada."*
> - *Cambiando la estructura: "La semana pasada vi una película que me encantó."*
>
> *Y bueno, volviendo a "Coco": ¿qué parte de la película fue la que más te hizo llorar?*

## Overview

Interlocutor runs two phases:

1. **A short check-in**, only on the first message of a fresh session (skipped entirely if a
   progress summary is available): target language, approximate level, and topics of interest.
   Claude infers a CEFR level (A1-C2) from however the user answers; no self-graded letter is
   required.
2. **An adaptive conversation loop**: Claude speaks only in the target language, reacts to what
   the user actually says, corrects real errors directly but kindly, always offers a few
   alternative phrasings (varying register, synonyms, and structure) even for correct sentences,
   and then continues. Difficulty drifts up or down based on how the last several turns went,
   without ever being announced.

A hard rule runs underneath all of it: Interlocutor never fabricates pronunciation feedback. Voice
mode hands Claude a text transcript, not audio, so any claim about accent or pronunciation would
be invented. Feedback stays limited to grammar, vocabulary, and textual fluency, always.

Because a portable skill can't assume any storage exists, progress lives in a plain-text summary
block the user copies out at the end of a session and pastes back in at the start of the next one.
Where a real filesystem is available, a local `progress.md` can serve as an optional fallback, but
the skill never depends on it.

## Version History

- **1.0.0** - Initial release.

## License

MIT
