---
name: kanciapa-mx5-scenes
description: Use when programming, verifying, or troubleshooting HeadRush MX5 rig/scene setup for the kanciapa guitar chain — block order, 5 Scenes, footswitch mapping, FX-Loop behavior questions
---

# HeadRush MX5 — rig & scenes (kanciapa)

Source of truth: `~/projects/music/kanciapa/kanciapa-notes.md` § "MX5 rig plan" and § "MX5 rear panel — corrected facts". This skill is a quick-reference/workflow layer on top of that file — always check it for the current state before acting, it changes as things get verified on-site.

## Signal chain (context)

Analog dirt (Fuzz/Redsmoke → TS Mini → DST Mini → Chorus) sits on the pedalboard, in front of Blackstar, already applied before SEND. MX5 sits in the amp's FX loop via its own **Guitar Input / Outputs** jacks (`Rig Input: Guitar`), not FX Ret. MX5's blocks work on an already-distorted signal — no dirt/distortion models needed on MX5.

## Rig: 10 fixed blocks, in order

| # | Block | Model (confirmed from official MX5 model card, saved at `~/projects/music/kanciapa/mx5-model-list.pdf`) |
|---|-------|------|
| 1 | Compressor | Gray Comp / Dyn III Comp |
| 2 | Octave | **Octave Pedal** (Boss OC-2, clean/mixable). Alt: Oct Fuzz (Dunlop Octavio, fuzz-flavored) |
| 3 | Pitch/Harmony | **Smart Harm** (auto-tracking harmonizer, no expression pedal needed). Alt: Wham/Harm/Chord Wham (need expression pedal) |
| 4 | Wah | Black Wah / Shine Wah / More Wah — all expression-pedal controlled |
| 5 | EQ | Graphic EQ / Para EQ |
| 6 | Rotary | Rotary |
| 7 | Phaser | Vibe Phaser (slow/fast presets recalled per Scene). Alt: Orange/Tron/Stone Phaser |
| 8 | Flanger | Flanger. Alt: Air Flanger |
| 9 | **FX-Loop** | Send→Vibratrem→DD2→Return, Mix (utility block, not a model) |
| 10 | Reverb | Eleven Reverb / Spring Reverb |

Order confirmed against [HeadRush 4-Cable Method guide](https://support.headrushfx.com/en/support/solutions/articles/69000801168-headrush-pedalboard-assigning-alternative-outputs-and-the-four-cable-method): block position determines what's inside vs. outside the FX-Loop.

**Auto-wah exists separately:** **Tron Filter** (Mu-Tron III Envelope Filter, dynamics-triggered, no expression pedal) — not used in any Scene currently, noted in case wanted later.

## 5 Scenes — the only things actually played live

Each Scene = on/off + preset snapshot across the 10 blocks, one footswitch press. **Not** one-block-per-scene (an earlier draft proposed 10 toggle-scenes; dropped, confirmed 2026-07-19 — see `kanciapa-notes.md` for the disambiguation note).

1. **Czysty/Rytm** — Compressor, EQ. FX-Loop off.
2. **High-gain z bramką** — analog dirt from pedalboard, Input Gate Thrsh tightened, EQ scoop, FX-Loop on (DD2 slapback). Compressor off.
3. **Solo — Rotary+Flanger** — Rotary, Flanger, EQ, FX-Loop on (DD2 long, Vibratrem off), Reverb.
4. **Solo — Phaser+Octave** — Phaser fast, Octave, EQ, FX-Loop on, subtle Reverb.
5. **Smart Harm+Wah ambient** — Smart Harm, Wah, EQ, Reverb (big wash). FX-Loop off/subtle.

Baseline params and per-scene deltas: see `kanciapa-notes.md` tables (subject to on-device ear-tuning — model card lists models, not per-parameter UI labels).

## Programming workflow (on-device)

1. Build the rig with all 10 blocks in the fixed order, `Rig Input: Guitar`. Dial in baseline params.
2. Model names are confirmed (table above) — no on-screen verification needed for which model to pick, only for exact parameter labels/ranges.
3. Build FX-Loop as a block (Send/Return/Mix params), not external cabling — physically re-cable Vibratrem + DD2 into MX5's FX Send/FX Return jacks. For scenes 1 and 5 (loop off), set **Mix=0%** rather than toggling the block Off — see open question below, this sidesteps it regardless of the answer.
4. For each of the 5 Scenes: toggle blocks on/off, apply per-scene deltas, save as a Scene snapshot within the one rig (not separate rigs).
5. Assign the 5 Scenes to MX5's physical footswitches.

## External footswitch control (Arduino 4-button + OLED)

Mismatch: 4 physical buttons, 5 Scenes. Needs a bank/shift button (hold = 5th scene) or OLED-driven scene selector + confirm button — not yet decided.

`mx5_pc_router.lua` (Reaper) is unrelated to this — it listens for Program Change **outgoing from MX5** (when a scene changes on-device) to auto-arm the Gitara/Bas track in Reaper. It does not send PC *to* MX5.

Open/unconfirmed: whether MX5 accepts incoming MIDI PC to recall Scenes (vs. only Rigs) — check `Global Settings` on-site.

## Resolved on-site (2026-07-20)

- **FX-Loop tone shift, root cause found:** toggling FX-Loop On/Off changed the tone (mid-range dip). Cause was **FX Return Level = Stomp**; setting it to **Rack** fixed most of it. Stereo/mono Mix setting was ruled out as the culprit. **Not fully resolved** — some residual tonal shift remains after the fix, not yet chased further.
- **Gate/Noise Filter blocks confirmed real** (2026-07-20, on-device screenshots) — MX5 has two separate insertable models, not just a global param: **Gate** (Threshold/Release/Noise Filter sub-param, on-device default −65dB/34ms/−120dB) and **Noise Filter** (dual low/high-band, Low/High Thresh/Attack/Release, default −20dB/−20dB/0.99ms/100ms). Plan: insert **Gate** as new block **#1**, ahead of Compressor (so compression doesn't amplify noise before the gate catches it, and gate doesn't chop delay/reverb tails later in the chain) — **not yet inserted**, do this next on-site. Noise Filter is the fallback if Gate alone isn't precise enough.
- **Rig already has an 11th block in use for Volume** (out of scope for the effects chain, kept as-is) — the "10 fixed blocks" table above is the effects chain only; once Gate is actually inserted, the effective count becomes 11 effect blocks + Volume. Update the numbered table once Gate is confirmed in place.

## Known open questions (verify on-site, don't assume)

- **FX-Loop bypass behavior:** does disabling block #9 (FX-Loop) break signal from earlier blocks (1-8, incl. Octave #2) reaching Main Out, or does it bypass cleanly like any other block? Still not directly confirmed (the 07-20 test found a tone-shift bug with a different root cause, not this). **Contingency that works regardless of the answer:** use Mix=0% instead of block Off for "loop off" scenes — dry always passes through independent of Return signal validity.
- Final Stereo/Mono choice for FX-Loop Mix (Vibratrem/DD2 are mono pedals) — ruled out as the tone-shift bug's cause, but the actual best setting for taste isn't finalized.
- Whether MX5 accepts incoming MIDI PC to recall Scenes (vs. only Rigs) — needed if the Arduino footswitch is to select scenes remotely.
