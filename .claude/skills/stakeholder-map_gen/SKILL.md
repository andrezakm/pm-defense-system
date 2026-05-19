---
name: stakeholder-map_gen
description: Builds a power/interest stakeholder map and operational personas from a PM's own exhaust — local files, cloud drive, email, meeting notes and chat. Plots every relevant person on a 2x2 grid and writes a persona for each, so any future content can be tailored to how that stakeholder expects to receive it.
when_to_use: When the user types /stakeholder-map_gen — to generate or refresh the stakeholder map for an initiative from connected channels and document folders.
---

# Skill: /stakeholder-map_gen

The baseline of the PM Defense System. Generates a stakeholder map and a set of
operational personas from data the PM already produces — no manual entry. The map
places every relevant person on a power/interest 2x2; the personas describe how each
key stakeholder communicates so that any later artifact can be polished to land with
them specifically.

## The 2x2 — axes and quadrants

**Vertical axis = power / influence.** Higher = more power and/or influence over the
initiative (decision authority, budget, headcount, seniority, others deferring to them).

**Horizontal axis = interest.** Left = low interest, right = high interest (how much
the person engages with, cares about, and drives the initiative).

Score each axis 0–10. The quadrant boundary is 5 on both axes.

| Quadrant | Power | Interest | Standard reaction |
|---|---|---|---|
| Manage closely / Key players | high | high | Active relationship. Co-create, involve in decisions, never surprise. |
| Keep satisfied | high | low | Powerful but disengaged. Keep them content; don't let them flip to caring at a bad moment. |
| Keep informed | low | high | Engaged but no power. Use as allies and sources; feed them information. |
| Minimal effort | low | low | Monitor only. Don't over-invest. |

The reaction labels follow the standard Mendelow grid. What matters is the semantics
of the two axes — power on the vertical, interest on the horizontal — not the labels.

## Process

### Step 1 — Ask the user about sources and scope

Before collecting anything, ask:

1. **Which sources?** Present what is realistically available and let the user pick:
   - **Local files** — a folder (or folders) of `.md` / `.txt` / other text docs (PRDs,
     decision docs, meeting notes). Ask for the path(s).
   - **Google Drive** — via the Drive connector, if connected.
   - **Email** — via the Gmail connector, if connected.
   - **Slack** — via the Slack connector, if connected.
   - **Meetings** — via a meeting/transcript connector (e.g. Granola), if connected.
   If a requested connector is not connected, say so plainly and continue with the rest.
2. **Which social groups / scope?** How should the relevant population be bounded —
   by company name(s), email domains, specific addresses, a Slack channel set, a project
   name? This filters who counts as a stakeholder for *this* initiative.
3. **Which initiative?** A short name/slug for the map being built (used in filenames
   and headings).

Do not proceed until sources and scope are clear.

### Step 2 — Collect the data

Pull the raw documents/messages from each chosen source within the scope:
- Local files: read every matching file in the given folder(s).
- Drive: search and read documents matching the scope.
- Gmail: search threads matching the company/domain/address scope.
- Slack: read the relevant channels / search messages matching the scope.
- Meetings: search by topic and by person. **If keyword search returns nothing, do not conclude the source is empty — enumerate the meeting folders and list meetings per folder.** Meeting series are often filed under folder names (e.g. a recurring 1:1, a daily standup) that no keyword query will match.

Keep a stable reference for every item (file path, email subject + date, Slack
channel + timestamp, Drive doc title) — citations depend on it.

### Step 3 — Per-document analysis, in parallel

Process documents in parallel — spawn Agent subagents (general-purpose), one per
document or per small batch, so extraction is concurrent and the main context stays
clean. Each agent analyses its document(s) and returns, **per person found**:

- **Person detection** — anyone who wrote, contributed to, or is named in the document.
  Normalise names/handles/email addresses to one identity where obviously the same person.
- **Sentiment** — overall tone only. One disposition per person in this document:
  `supportive` / `neutral` / `skeptical` / `hostile` / `mixed`.
- **Power level** — 0–10. Signals: seniority/title, decision authority (approves, signs
  off, allocates budget or headcount), others deferring to or waiting on them, setting
  direction, overriding others.
- **Interest level** — 0–10. Signals: frequency and depth of engagement, initiating vs.
  only reacting, asking follow-up questions, emotional investment in the outcome.
- **Citations** — for every important finding (a score, a tone call), keep a short
  verbatim quote and its source reference. Insights must be anchored, not asserted.

### Step 4 — Synthesize

Merge the per-document findings into one record per person:
- Reconcile scores across documents into a single power and interest score. Note any
  trajectory (tone improving or worsening over time, interest rising or falling).
- Resolve contradictions explicitly rather than averaging them away.
- Assign a **confidence** level from how much data backs the person:
  low (<3 items), medium (3–8), high (>8). Thin data is expected — be honest about it.
- Drop people who are merely mentioned in passing with no signal; keep anyone with a
  genuine stake.

### Step 5 — Build the map and personas

Place every retained person on the 2x2 by their (interest, power) scores. Write a
persona for each. Then produce both outputs (Step 6).

## Persona format

Each persona is operational: its purpose is to let later content be tailored to the
stakeholder. Fields:

- **Name / identifier**
- **Role** — title or function, inferred if not explicit
- **Group** — company / org / team
- **Power** — score 0–10 + one-line rationale
- **Interest** — score 0–10 + one-line rationale
- **Quadrant** — which of the four, and the reaction strategy
- **Overall tone** — sentiment label + trajectory if visible
- **Communication style** — how they actually write (terse / detailed, formal / casual,
  data-led / narrative-led, fast / slow to respond)
- **Cares about** — recurring themes, priorities, what they keep returning to
- **What moves them** — the kind of evidence that lands (hard numbers, customer quotes,
  exec backing, cost/risk framing, a crisp recommendation, …)
- **Expected packaging** — how to deliver content so it lands: length, channel, tone,
  structure
- **Under pressure** — how they react when challenged or when things go wrong
- **Confidence** — low / medium / high, with the reason
- **Citations** — source anchors for the load-bearing claims

## Step 6 — Outputs

Write to `./stakeholder-maps/YYYY-MM-DD-<initiative-slug>/` relative to the working
directory (`pwd`). Timestamp via `date '+%Y-%m-%d'`.

**`stakeholder-map.md`** — the report:
1. Header: initiative, date, sources used, scope.
2. The four quadrants, each listing its people with one-line summaries.
3. Full personas, one section each, in the format above.
4. A short honest note on data coverage and where confidence is low.

**`stakeholder-map.html`** — a static visual. Produce **one fully standalone HTML file**:
all CSS inlined, no external assets — **no web fonts, no CDN links** — use a system font
stack so the file works fully offline. Use a clean, neutral, professional style —
generous whitespace, a restrained palette, clear typographic hierarchy, no emoji.
Key requirements:
- A 2x2 grid: power on the vertical axis (high at top), interest on the horizontal
  (high at right). Label all four quadrants with their reaction.
- Plot each person at their (interest, power) coordinate as a small marker with the name.
- Persona cards below the grid.
- **Design system (optional):** if the user has pointed you at a design system — brand
  colours, fonts, spacing tokens, or example templates — read those files and follow
  them instead of the neutral default. Otherwise use the neutral default; do not invent
  a brand.

## Step 7 — Confirm

Report briefly: output folder, how many people mapped, the quadrant split, and an
explicit flag of which personas rest on thin data.

## Notes

- The skill only reads sources; it never writes back to email, chat or the cloud drive.
- When the source corpus is thin, expect low-confidence personas — say so rather than
  inventing detail.
- Re-running for the same initiative refreshes the map; keep each run in its own
  dated folder so trajectory stays visible.
