---
name: transpose_gen
description: Transposes an existing document into the material a PM needs to run a specific communication channel — a slide presentation for a meeting, a script for a video message, a long-form report, or a chat message. Reads the stakeholder map and communication plan to tailor content to that channel's blended audience, and outputs a navigable, fully standalone HTML deck.
when_to_use: When the user types /transpose_gen — to convert an original document into channel-ready material for a channel defined in the communication plan.
---

# Skill: /transpose_gen

Takes a document and re-casts it as the **material the PM needs to execute one
communication channel**. Not a generic re-format — a defense move: the right content,
at the right depth, in the form and tone that channel's audience expects.

## Inputs

From the user:
1. **The original document** — a file path (`.md`, `.txt`, `.docx`, `.pdf`) or pasted
   text. The information to be transposed.
2. **The target channel** — the name of a channel from the communication plan
   (e.g. "the steering committee", "the weekly working sync", "video message").

If either is missing, ask for it. Everything else the skill resolves itself.

## Hard dependencies

This skill **reads, and cannot run without:**
- the most recent `stakeholder-map.md` under `./stakeholder-maps/`
- the most recent `communication-plan.md` under `./communication-plans/`

If either is missing, stop and tell the user to run `/stakeholder-map_gen` and/or
`/communication-plan_gen` first.

## Process

### Step 1 — Gather inputs
Get the original document and the target channel from the user.

### Step 2 — Load the channel context
- In `communication-plan.md`, locate the named channel. Pull its **type**, **members**,
  **cadence**, **treatment recommendations**, and **defensive purpose** ("defends
  against"). If the channel name is ambiguous, ask the user which one they mean.
- In `stakeholder-map.md`, pull the **persona** of every member of that channel —
  communication style, tone, what moves them, expected packaging, under-pressure
  behaviour, confidence.

### Step 3 — Determine the artifact form
Always deliver the material the PM actually needs to *prepare and run* that channel.
The output is always one navigable HTML file, but its internal form follows the channel:

| Channel type | Artifact form |
|---|---|
| Regular 1:1 · steering body · working sync · occasional meeting | A **slide presentation** — the material to present and talk through in the meeting. |
| Regular report | A **long-form sectioned report** — written to be read, not presented. |
| Video message | A **video script** (Loom-style) — laid out as beat/cue cards to advance through while recording. |
| Slack message | The **Slack message itself** — short and ready to paste; any rationale or backup on later screens. |

### Step 4 — Transpose the content
Re-shape the original document for the channel:
- **Select & compress to fit.** Decide what to keep, what to cut, and what to lead with,
  given the channel's appropriate depth — a video message stays short, a report stays
  full, a meeting deck stays focused, a Slack message is minimal. Do not dump the whole
  original into a channel that doesn't warrant it.
- **Tailor to the blended audience.** Write for the channel's members as a group — open
  with what they collectively care about, lead with what moves them, in the tone, length
  and packaging their personas expect. (A high-leverage decision-maker who needs bespoke
  handling already has their own dedicated channel — so a group channel can safely be
  treated as one blended audience.)
- **Honour the channel's treatment recommendations** from the communication plan
  verbatim — e.g. "this group skews skeptical — open with evidence, not vision."
- **Keep the defensive purpose in view.** The channel's "defends against" line is the
  thing this document must not fail to do.

### Step 5 — Build the HTML
Produce **one fully standalone, navigable HTML file** — built from scratch, with all CSS
and a small keyboard-navigation script inlined. No external stylesheet, no external
runtime, no shared assets, **no web fonts or CDN links** — use a system font stack so the
file works fully offline. The file must open and present correctly on its own and be
shareable as a single attachment. Never split a deck across files.
- **Navigation.** Each idea is one screen ("slide"). A short inlined script shows one
  screen at a time and advances / rewinds on ← / → (also Space, PageUp / PageDown), with
  a visible slide counter. Keep it minimal and dependency-free.
- **Form follows channel.** Slide-form for meetings; for report / script / message forms
  each section or beat is one screen — the same navigable container, different density
  per screen.
- **Style.** Use a clean, neutral, professional style — restrained palette, clear
  typographic hierarchy, generous whitespace, no emoji. Match the tone of the source
  document, including its language. **Design system (optional):** if the user has
  pointed you at a design system — brand colours, fonts, tokens, example deck templates —
  read those files and follow them instead of the neutral default.

### Step 6 — Output
Write to `./channel-docs/YYYY-MM-DD-<channel-slug>/` relative to the working directory.
Timestamp via `date '+%Y-%m-%d'`. Each artifact is a single standalone HTML file with a
descriptive filename (`<channel-slug>-<doc-slug>.html`) — no companion assets. If a run
produces several artifacts, each is still its own self-contained file.

### Step 7 — Confirm
Briefly: which channel, which artifact form, and what was cut or compressed and why.

## Notes

- This is a defense artifact, not a summary. Every choice — what leads, what is cut, the
  tone — serves the channel's audience and the channel's defensive purpose.
- If the original document is thin or off-topic for the named channel, say so rather
  than padding.
- The skill only reads its inputs and writes the channel-doc folder; it changes nothing
  in email, Slack or calendars.
