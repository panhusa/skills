---
name: kanciapa
description: Use whenever working on the kanciapa project — Dawid's jam/recording room (~/projects/music/kanciapa/): guitar+bass+drums setup, REAPER, MX5, band-iem (personal IEM monitoring), room acoustics/treatment, or the wireless-guitar/Aria/beltpack hardware project. Also use before trusting any claim in kanciapa's docs about hardware or machine state, since this project has a real history of docs going stale and contradicting each other — even when the user doesn't explicitly ask for orientation.
---

# kanciapa — orientation

This project has many interrelated docs and a real history of them going stale
silently — most recently, three docs kept describing a REAPER/Ubuntu setup on
a laptop that had been wiped to Windows-11-only for months. Don't trust a
doc's claim just because it reads as settled fact; check it against the most
recently touched related file first, especially for anything hardware- or
machine-state-related.

## Read first, every time

**`PROJECT.md` and `kanciapa-notes.md`** — PROJECT.md itself names these the
authoritative living docs. Read both in full before acting on anything else
in this project, even if the task seems narrow (grepping for a keyword and
acting on one match is exactly how staleness gets missed).

## Subsystem map

Don't confuse these — they're related but distinct, with their own docs and
sometimes their own hardware:

| Subsystem | What it is | Docs |
|---|---|---|
| `kanciapa/` (root) | The physical room: audio routing, REAPER, MX5, live rig, recording | `PROJECT.md`, `kanciapa-notes.md`, `SETUP.md`, `kanciapa-todo.md`, `plans/*.md` |
| `kanciapa/band-iem/` | Separate app, own git repo — personal IEM mixing for musicians' phones. Different audience/hardware than the room's recording setup: UR44 + Fast Track Ultra via VB-Audio Matrix, hosted on `bunkier` | `band-iem/PROJECT.md` |
| `kanciapa/acoustics/` | Room measurement (REW) and treatment placement — separate concern, ties into band-iem's future Control Room EQ but not started | `acoustics/PROJECT.md` |
| Wireless guitar / Aria / beltpack | Aria guitar hardware project — ESP32 MIDI + wireless audio, its own thing | `wireless-guitar-plan.md`, `guitar-passive-wiring.md`, `beltpack-design.md` |

Before assuming a decision in one subsystem applies to another (e.g. "the
interface" or "the laptop"), check which subsystem's docs actually describe
the thing you're touching — they can disagree, and the more recently edited
one usually wins, but confirm rather than assume.

## The bunkier lesson

`kanciapa-notes.md`, `SETUP.md`, and `reaper-mcp-setup.md` all describe the HP
EliteBook 840 G5 as an Ubuntu 24.04 box running REAPER at `192.168.1.70` —
this was true when written, and is no longer true. That G5 is now `bunkier`:
Windows 11 only, Ubuntu wiped. None of those docs were updated when it
happened; it surfaced only when a brand-new doc (`acoustics/PROJECT.md`)
mentioned it in passing and the two didn't match.

Check `project_bunkier.md` in memory (or just ask) for the current state of
that machine before relying on any doc's description of "the G5" or "the
laptop" — the same blind spot can recur for other hardware facts too (an
interface, a cable, a pedal's role), so treat any specific hardware/machine
claim as worth a quick sanity check if the task depends on it being right.

`~/.claude/skills/laptop-ubuntu-kanciapa/` already documents this — it has
its own STALE banner (2026-09-19) and is kept for Ubuntu-era reference only,
not current instructions.

## Related skills — use these instead of re-deriving their scope here

- **MX5 rig/scene programming, footswitch mapping, FX-Loop questions** →
  `kanciapa-mx5-scenes` skill. Current and well-maintained, don't duplicate it.
- **Auditing/consolidating/reviewing kanciapa docs for contradictions,
  staleness, or duplicate tracking** → `spec-coherence-review` skill. It's
  general-purpose and already proven on this exact project (caught the
  DPDT contradiction, a broken cross-reference, and duplicate TODOs
  2026-09-21) — don't hand-roll a consistency check here.
- **Planning new recording/hardware/architecture work** →
  `superpowers:brainstorming` skill, same as any other project.

## Known-stale patterns to watch for (not exhaustive — just what's bitten before)

- A doc marked "implemented ✅" or "Status: X" with no date, next to other
  docs that have since moved past it — check the date and cross-reference
  before trusting the status label itself.
- Shopping lists / "buy X" items that duplicate across `PROJECT.md` and
  `kanciapa-todo.md` at different granularity — one can get checked off
  without the other updating.
- MX5 routing/architecture claims specifically — this has been redesigned
  several times (DPDT dual-instrument → guitar-only-in-loop), and old
  docs describing the earlier architecture don't always say so.
