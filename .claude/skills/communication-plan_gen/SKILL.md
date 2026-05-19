---
name: communication-plan_gen
description: Turns a stakeholder map into a pragmatic, defense-minded communication plan — assigns every stakeholder to a fitting information channel (video message, chat message, report, occasional meeting, regular meeting), names any 1:n meeting that is really a steering body, and writes per-channel handling recommendations grounded in the personas. Outputs a markdown plan and a standalone HTML view.
when_to_use: When the user types /communication-plan_gen — to convert an existing stakeholder map into a channel-by-channel communication plan.
---

# Skill: /communication-plan_gen

Takes the stakeholder map and answers one question: **through which channel does the PM
keep each stakeholder informed and engaged — for the least effort that still keeps no
dangerous person blindsided?**

This is a defense instrument. The goal is not communication for its own sake. It is to
spend the PM's expensive attention (1:1s, meetings) only where power and stakes demand
it, and to route everyone else into low-maintenance channels they can opt out of —
while making sure nobody with the power to hurt the initiative is left in the dark.

## Hard dependency — the stakeholder map

This skill **consumes a stakeholder map and cannot run without one.** At the start, find
the most recent `stakeholder-map.md` under `./stakeholder-maps/` (relative to the working
directory). If none exists, stop and tell the user to run `/stakeholder-map_gen` first.

Read it **completely** — every quadrant, every power/interest score, every persona
(communication style, tone, what moves them, under-pressure behaviour), every confidence
level, the group labels, the structural-only table, and the honest-read notes. The
channel assignment is only as good as the persona detail behind it.

## The channels

Six channels, ordered here from **highest friction / highest importance** (top) to
**lowest** (bottom):

| Channel | What it is | Typical fit |
|---|---|---|
| **Regular 1:1 meeting** | A standing one-to-one slot. | The few highest-power / highest-stakes / highest-friction individuals — people you cannot afford to manage through a group. |
| **Regular group meeting (1:n)** | A standing group meeting. **Two kinds — name them differently:** | see below |
| &nbsp;&nbsp;· *Steering body* | A 1:n of high-power people who actually steer the initiative. Give it a fitting name (e.g. "Steering Committee", "Strategy Sync") and treat it as its own category. | High-power people who decide. |
| &nbsp;&nbsp;· *Informational standing meeting* | A low-leverage, low-maintenance keep-informed group. | People who should not feel blindsided but do not steer. |
| **Occasional meetings** | A real conversation now and then, no standing slot. | People whose interest spikes intermittently; many "keep satisfied" stakeholders. |
| **Regular report** | A written, periodic, push update. | Engaged-but-lower-power people; satisfied-but-disengaged people who want to stay informed without taking your time. |
| **Slack message** | Light, ad hoc, push. | Low-touch informing. |
| **Video message** | Lowest friction; async broadcast. | Minimal-effort stakeholders; a broad keep-warm. |

## Process

### Step 1 — Load and absorb the stakeholder map
As above. Do not proceed without it.

### Step 2 — Channel-first assignment pass
Do **not** go person by person here. Go **channel by channel, highest friction first** —
Regular 1:1 → Steering body → Informational 1:n → Occasional meetings → Report → Slack →
Video. For each channel, ask: *who genuinely belongs here?*

- Fill the expensive channels deliberately and sparingly. A 1:1 costs real time — it is
  earned by power, by stakes, or by friction (a tense relationship, an unresolved fault
  line, a skeptical or mixed-tone high-power person a group channel cannot safely hold).
- Quadrant is the starting signal (A → high touch … D → low touch) but persona overrides
  it: tone, communication style and confidence decide the final fit.
- When you place a 1:n group meeting, decide its **purpose** — steering or informational —
  and name it accordingly. A steering body is its own category; do not mislabel a
  decision forum as a "keep-informed" meeting.

### Step 3 — Layering pass
Each stakeholder has **one primary channel** — the highest-touch channel they genuinely
need. On top of that, high-leverage people **may** also be added to a broadcast channel
(report / Slack / video) as **opt-out** recipients — but only where it is pragmatic and
low-maintenance. The test is realism: if the plan has the PM running six standing
meetings, it is wrong. Keep it manageable.

### Step 4 — Stakeholder recheck pass
**Now** go person by person through the *entire* stakeholder list — every persona and
every row of the structural-only table. For each, confirm the channel fits their power,
interest, tone and persona. Hunt for misfits and fix them:
- High-power person in a broadcast-only channel → upgrade.
- Low-interest person occupying a 1:1 → downgrade; reclaim the PM's time.
- Skeptical / mixed-tone high-power person hidden in a group channel → consider a 1:1.
- Low-confidence stakeholder → assign conservatively and say so.
- Anyone missing → assign.
Record every change and the reason.

### Step 5 — Draft the final plan
For each channel that ended up with people in it, state:
- **Channel name** — for a 1:n, the purpose-fitting name (steering body vs. informational).
- **Cadence** — a concrete recommendation (e.g. weekly, bi-weekly, monthly, quarterly, ad hoc).
- **Members** — who is in it; mark opt-out broadcast members distinctly from primary members.
- **How to treat this channel** — concrete handling recommendations grounded in the
  *personas and tones of the people actually in it*. Examples of the right altitude:
  "this group skews skeptical — open with evidence, not vision"; "the decider here reads
  fast and wants one recommendation — never bring three options."
- **What it defends against** — the defensive purpose: the specific way the PM gets hurt
  if this channel does not exist.

### Step 6 — Outputs
Write to `./communication-plans/YYYY-MM-DD-<initiative-slug>/` relative to the working
directory. Timestamp via `date '+%Y-%m-%d'`; reuse the initiative slug from the
stakeholder map.

**`communication-plan.md`** — a simple list: each channel with its name, cadence,
members, treatment recommendations, and defensive purpose. Plus a short opening note on
which stakeholder map it was built from, and a short closing note on what the recheck
pass changed.

**`communication-plan.html`** — a static HTML view of the same plan. Produce **one fully
standalone HTML file**: all CSS inlined, no external assets — **no web fonts, no CDN
links** — use a system font stack so the file works fully offline. Use a clean, neutral,
professional style — restrained palette, clear typographic hierarchy, no emoji; one
clear block per channel. **Design system (optional):** if the user has pointed you at a
design system — brand colours, fonts, tokens, templates — read those files and follow
them instead of the neutral default.

**Calendar view — both files end with one.** After the channel list (and recheck note),
add a calendar view so the PM can see the *rhythm and load* at a glance:
- A **"typical week"** grid (Mon–Fri) that places every recurring channel-meeting on the
  day and rough time it would run — daily steering, weekly 1:1s and syncs, bi-weekly
  items marked as alternating. Use plausible times; if the stakeholder map or source
  meetings reveal real cadences, reuse them.
- A separate **cadence strip** for the non-weekly items — monthly report, monthly video,
  quarterly board sessions, ad-hoc Slack / occasional meetings.
The point is a single glanceable picture that confirms the plan is sustainable — it is
the visual form of the realism test.

### Step 7 — Confirm
Briefly: how many channels are active, how the stakeholders split across them, and the
notable moves from the recheck pass.

## Notes

- The whole plan is judged on **realism**. A plan the PM cannot actually sustain is a
  defense that fails on contact.
- Be honest about low-confidence stakeholders — assign them a cheap channel and say the
  read is provisional.
- The skill only reads and writes plan files; it changes nothing in email, Slack or
  calendars.
