---
name: av-signal-chain
description: Help with audio (and AV) signal-chain engineering — pedalboards, patchbays, FX loops, DI boxes, gain staging, buffered vs true-bypass, and hum/buzz/ground-loop diagnosis. Use this whenever Dawid is wiring, debugging noise on, or planning any part of the kanciapa signal chain (guitar/bass/amp/MX5/pedals/interfaces) or a similar personal AV setup — even if he doesn't say "signal chain" explicitly, e.g. "why does my amp buzz", "how do I build a pedalboard", "should I add a buffer", "how do I wire a patchbay", "what DI should I get", "is this cable run OK". Goal: consistent, professional-sounding signal every time, not a one-off fix.
---

# AV signal-chain engineering

Dawid runs a home rehearsal room (kanciapa: guitar/bass/drums, Blackstar HT20R amp with FX loop, HeadRush MX5, pedalboard, Saffire Pro 40 / Scarlett interfaces, Reaper). This skill captures the reasoning patterns that actually found and fixed problems there, so future sessions don't start from zero.

Full case history and live diagnostic log: `~/projects/music/kanciapa/plans/2026-07-17-iem-loop-monitoring-design.md` and `~/projects/music/kanciapa/kanciapa-notes.md`. Check these first for anything already tried/decided before proposing something new.

## Core mental model: two separate networks

Every rig has two overlapping networks that both terminate at device chassis:
1. **The audio-signal network** — jack/XLR/MIDI cables, whose shields tie chassis together electrically.
2. **The mains-safety-ground network** — every device's PE (earth) pin, which also ties chassis together, through the wall wiring.

Both networks are required and neither can be "disconnected" for safety reasons. Problems (hum, buzz, noise) come from **impedance differences between these two networks for any pair of devices connected by both** — i.e. any two devices linked by an audio cable, where the mains-PE path between them isn't short/low-impedance. This reframe matters: don't think "is X connected to Y" (it always is, eventually) — think "how much impedance is between X and Y, on the path that matters."

### Star power topology

For any pair of devices linked by an audio cable, minimize the number of extension-cord/strip junctions between them on the mains side. Practical rule: one central power strip, every device plugged directly into it (a "star"), not chained strip-into-strip. If a device (e.g. a pedalboard) has its own onboard power strip feeding several PSUs, that's an acceptable single hop — the point is to avoid *stacking* multiple avoidable hops, not to eliminate the one hop that's structurally necessary.

Devices with no audio/MIDI/USB cable to the rest of the rig (phone chargers, standalone LED lights) don't participate in this loop mechanism at all and can be wired however's convenient — this distinction (audio-connected vs. standalone-electrical) is often what actually simplifies a "how do I power everything" question.

### Diagnosing with a multimeter, not just by ear

- **PE continuity resistance** (mains unplugged): probe PE pin to PE pin between two devices through their actual cable run. Should be a fraction of an ohm; an elevated reading pinpoints which junction is bad.
- **AC voltage between chassis** (mains on, the definitive ground-loop test): multimeter on AC volts, probe the metal chassis/ground point of each of the two devices. A nonzero reading (tens–hundreds of mV) confirms and quantifies a ground loop between that specific pair. Measure before/after a change for an objective comparison instead of relying on ear alone.

### One variable at a time

When debugging noise, change exactly one thing per test and re-listen (or re-measure) before changing the next. Order changes cheapest/fastest first: rewiring power (minutes, free) before buying an isolation DI (cost, last resort). Narrow the reproducible case as far as possible first (e.g. "does it hum with just the amp + one device in the loop, before adding the rest of the pedals?") — a smaller reproducible case is faster to iterate on and easier to reason about.

## Gain staging and levels

Mismatched levels (instrument/Hi-Z vs. line) between two connected devices cause either excess noise floor (input too quiet, gained up later) or clipping/distortion (input too hot). Before blaming grounding, check: is each device's input actually set for the level the previous device outputs? (e.g. an audio interface set to "Instrument" when fed a line-level signal, or vice versa.) This is a distinct failure mode from ground loops and sounds different — clipping/distortion/thin sound rather than a steady 50/100Hz hum — but is easy to conflate when troubleshooting a "bad sound" complaint in general.

## Buffers, true bypass, and cable runs

- Passive guitar pickups are high-impedance sources; long cable runs (typically >~6m, or several meters through multiple true-bypass pedals in series) act as a low-pass filter against that impedance, dulling the top end ("tone suck") and raising noise susceptibility.
- **True bypass** pedals pass the signal through unbuffered when off — fine individually, but several in series add up the cable capacitance between them.
- **Buffered bypass** pedals (or a dedicated buffer pedal) lower the source impedance, which fixes tone suck and reduces noise pickup over long runs — but a bad/cheap buffer can add its own noise or coloration, and putting a buffer before a fuzz pedal often breaks the fuzz's interaction with guitar volume/tone (fuzz circuits are notoriously interactive with the raw pickup impedance).
- Rule of thumb: one buffer early in the chain (but usually after any fuzz) is normally enough for a typical pedalboard; more isn't automatically better.

## Patchbays and DI boxes

- **Normal/half-normal/parallel** switching on a patchbay jack determines whether patching into the front breaks the internal (rear-to-rear) connection or just taps it. Half-normal = tapping without breaking the existing signal path; open/parallel = always broken/always both; know which mode a given channel is in before relying on it — this is a common source of "why is there no signal" or "why is it doubled" confusion.
- A **DI box** converts unbalanced instrument level to balanced line level (XLR) and often provides ground lift — but ground lift behavior is not always intuitive (it can make hum worse in some topologies, as found in the kanciapa case), so treat it as a variable to test, not an assumed fix.
- An active/tone-shaping DI (e.g. one with a footswitchable drive/EQ stage) is not automatically equivalent to a clean isolation DI even in bypass mode — if a build specifically needs ground-loop-breaking duty, a purpose-built transformer-isolated passive DI is the more reliable tool.

## When advising

- Check the kanciapa project files (linked above) for anything already tried/decided before suggesting a new experiment — don't re-propose something the log says is already ruled out.
- Prefer free/fast changes (rewiring, settings) before recommending a purchase.
- Separate "remote planning/decisions" from "on-site execution" explicitly in any plan — on-site steps in a rehearsal room are often 5-minute jobs once decided, but deciding what to test/buy needs to happen beforehand. Batch on-site steps into a single ordered checklist for one visit rather than spreading them across many.
- Stay scoped to what was actually asked — note tangential ideas (e.g. unrelated safety upgrades) briefly if genuinely relevant, but don't build them out unless asked.
