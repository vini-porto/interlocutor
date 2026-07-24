# Interlocutor

A Claude Skill that turns a chat, including voice mode, into a foreign-language conversation
partner. It's plain Markdown, so it works anywhere Claude Skills work: no code, no build step, no
external dependencies.

## Installation

Add `SKILL.md` as a Skill in a Claude app that supports voice mode:

1. Open **Settings → Capabilities → Skills** (naming may vary slightly by platform/version).
2. Create a new skill and paste in the contents of `SKILL.md`, or upload the file directly if your
   client supports file upload for skills.
3. Start a new chat (or voice chat) and it's available.

If your client doesn't support custom Skills yet, you can get the same behavior in a Claude
Project: paste the contents of `SKILL.md` into the Project's custom instructions.

## Usage

Open a new chat (or voice chat) and just start talking. You don't need to fill out anything up
front, Claude will ask.

```
Let's practice a language.
```

You can also front-load what you already know to skip past the parts of the check-in you'd rather
not repeat:

```
I want to practice Portuguese conversation. I'm somewhere around B1, and I like
talking about travel and music.
```

In voice mode, feedback stays limited to grammar, vocabulary, and phrasing, since Claude only ever
receives a text transcript of what you said, never the audio. Interlocutor never invents an
opinion about your accent or pronunciation; see [The Pronunciation
Rule](SKILL.md#the-pronunciation-rule) in `SKILL.md`.

### Resuming a session

At the end of a session, Claude prints a short plain-text progress summary. Save it somewhere (a
note, a Project's persistent knowledge, whatever's convenient), and paste it back in as your first
message next time:

```
INTERLOCUTOR PROGRESS SAVE
Language: Portuguese
Level: B1, upper end
Topics covered: travel, weekend plans, ordering food
Recurring errors: subjunctive after "quando", gender agreement on adjectives
Strengths: good vocabulary range, comfortable with past tense narration
Notes: prefers informal register, enjoys travel and music topics
```

Claude reads it, skips the check-in, and picks the conversation back up from there. This
copy-paste block is the only continuity mechanism the skill relies on, since a chat has no
guaranteed memory of previous sessions.

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

Since a chat has no guaranteed memory across sessions, progress lives in a plain-text summary
block the user copies out at the end of a session and pastes back in at the start of the next one,
preserving context without depending on any particular storage.

## Version History

- **1.0.0** - Initial release.

## License

MIT
