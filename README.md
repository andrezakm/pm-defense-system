# PM Defense System — Skills

A small set of Claude Code skills that automate the **articulable half of stakeholder
management** for a product manager.

They do not do the PM's job. They do the part that *can* be written down — mapping who
matters, planning how to reach them, and re-casting a document for a specific audience —
fast, and as defensible artifacts. What is left for the PM is the part that cannot be
written down: the judgment in the room. The skills reclaim the preparation; they
deliberately leave the meeting.

Used together they cover a meaningful slice of a PM's recurring coordination load —
roughly the stakeholder-management portion — and turn it from invisible effort into
visible, inspectable output.

> This is the **portable edition**. It has no dependency on any company design system —
> every HTML output is a clean, self-contained file. See *Output styling* below.

> **The session is part of the tool.** Once a stakeholder map and a communication plan
> have been generated in a Claude Code session, they sit in that session's context. You
> can then simply *talk to that session* about anything stakeholder-related — "how do I
> handle X in the next steering meeting?", "who will resist this decision and why?",
> "draft a note for Y" — and Claude answers grounded in the map and the plan. The skills
> produce the artifacts; the loaded session becomes an ongoing stakeholder advisor.

## Install

Copy the three skill folders into your project's `.claude/skills/` directory:

```
.claude/skills/stakeholder-map_gen/
.claude/skills/communication-plan_gen/
.claude/skills/transpose_gen/
```

Then invoke them in Claude Code as `/stakeholder-map_gen`, `/communication-plan_gen`,
and `/transpose_gen`.

## The three skills

They form a chain. Each builds on the one before.

| Skill | What it does | Reads | Produces |
|---|---|---|---|
| `/stakeholder-map_gen` | Builds a power/interest map of everyone around an initiative, with an operational persona for each key person. | Your email, calendar/meeting notes, cloud documents, local files. | A markdown map + an HTML view, under `stakeholder-maps/`. |
| `/communication-plan_gen` | Turns the map into a channel plan — who hears what, through which channel, how to handle it, on what cadence. | The latest stakeholder map. | A markdown plan + an HTML view (with a calendar), under `communication-plans/`. |
| `/transpose_gen` | Re-casts an existing document into the material one channel needs — a slide deck, a video script, a report, or a message. | The latest map + plan, and a document you provide. | A standalone HTML artifact, under `channel-docs/`. |

`/communication-plan_gen` will not run without a stakeholder map. `/transpose_gen` will
not run without both. Run them in order the first time.

## What you need

- **Claude Code**, with the three skill folders placed in `.claude/skills/`.
- **Source material** for `/stakeholder-map_gen` — any mix of:
  - connected channels: email, calendar / meeting-notes, a cloud drive, chat;
  - local folders of `.md` / `.txt` / other documents.
  - The skill asks you which you have at the start; it works with whatever is available.
- No git repository is required to *run* the skills.

## Sample flow

A PM wants to get ahead of stakeholder management for an initiative — say a platform
migration.

**1. Map the stakeholders.**
```
/stakeholder-map_gen
```
The skill asks three things: which sources to use, how to scope the people
(by company, email domain, project name, a channel set), and a short name for the
initiative. It then collects the material, analyses each document, and writes a map to
`stakeholder-maps/2026-05-19-platform-migration/` — a markdown report and an HTML 2x2.
Open the HTML, read the personas, correct anything that looks wrong.

**2. Plan the communication.**
```
/communication-plan_gen
```
It reads the map you just made and sorts everyone into communication channels —
1:1s, steering meetings, working syncs, reports, lighter touchpoints — filling the
high-effort channels first and only where power or friction earns it. Output lands in
`communication-plans/2026-05-19-platform-migration/`, including a calendar view of the
recurring load. Adjust any placement you disagree with.

**3. Transpose a document for a channel.**
```
/transpose_gen
```
Give it a document (a strategy memo, a decision doc) and the name of a channel from the
plan — e.g. "the steering committee". It pulls that channel's audience and handling
notes, selects and compresses the content to fit, and writes a single self-contained
HTML file to `channel-docs/` — a slide deck for a meeting, a script for a video message,
a report for the report channel, a message for a chat channel.

Repeat step 3 for each channel you need to feed. Repeat steps 1–2 whenever the initiative
or the people around it change.

## What you get

```
stakeholder-maps/2026-05-19-<initiative>/    stakeholder-map.md   · stakeholder-map.html
communication-plans/2026-05-19-<initiative>/ communication-plan.md · communication-plan.html
channel-docs/2026-05-19-<channel>/           <channel>-<doc>.html
```

Every HTML file is **standalone** — styling and any interactivity are inlined, so a file
can be opened, presented, or emailed on its own with nothing else attached.

## Output styling and design systems

By default the skills emit clean, self-contained HTML with a neutral built-in style.
Nothing to set up — the files just open and look presentable.

If your organisation has a **design system** — brand colours, fonts, spacing tokens, or
example templates — you can have the skills match it instead:

- **At runtime**, just tell Claude where it is: *"...and style the HTML using the design
  system in `./brand/`"* — Claude will read those files and apply them.
- **Permanently**, edit the HTML-output step inside each `SKILL.md` to point at your
  design-system files, so every run follows your brand without being asked.

A design system is optional. The skills are fully functional without one.

## Notes

- **Confidence is reported, not hidden.** Where the source material is thin, the map and
  plan say so rather than inventing detail. Treat low-confidence entries as provisional.
- **The map reflects a vantage point.** It is drawn from the perspective of whoever's
  sources are used. A PM mapping from inside an org sees peers and reports clearly; an
  outside advisor sees leadership clearly. The output is shaped by the input — read it
  with that in mind.
- The skills only read your sources and write the output folders. They do not send mail,
  post to chat, or change calendars.
