---
name: sound-physics
description: Ground practical acoustics decisions in real physics — wave propagation, room modes/standing waves, absorption vs. diffusion, Helmholtz resonance, inverse-square law, near/far field, speaker and mic directivity, and phase/comb filtering when summing mics. Use this whenever Dawid is placing acoustic panels or bass traps, positioning a mic on a cab/drum/room, placing or aiming monitors/speakers, reading a REW frequency-response graph, reasoning about a room mode or a dip/peak in a measurement, deciding mic spacing for a two-mic source, or asking "why does it sound like X here" about the kanciapa room — even if he doesn't say "acoustics" or "physics" explicitly, e.g. "where should the bass trap go", "why is there a null at this spot", "how far should the monitors be from the wall", "will these two mics cancel each other out". This is the physical/acoustic counterpart to the av-signal-chain skill (which covers electrical grounding, gain staging, buffers) — use this one for anything about how sound waves themselves behave in air and in the room, not the electrical signal chain.
---

# Sound physics — acoustics and fluid mechanics of sound

Dawid's kanciapa room (5.5m × 5.5m × 3.5m, nearly square footprint) is getting measured and treated — see `~/projects/music/kanciapa/acoustics/PROJECT.md` for the live project (REW measurement workflow, ECM8000 mic, panels + corner bass traps). This skill captures the physics that should drive those decisions, so recommendations are derived from first principles instead of borrowed rules of thumb that may not fit this specific room.

Sound in air is a **fluid-mechanics problem**: pressure waves propagating through a compressible medium. Every acoustic phenomenon below — modes, absorption, diffraction, directivity — falls out of that one fact. When something in the room behaves oddly, first ask "what is the air actually doing here" before reaching for a rule of thumb.

## Core constants and formulas

- **Speed of sound in air:** c ≈ 343 m/s at 20°C, rising ~0.6 m/s per °C. Room-temperature swings in kanciapa are small enough to ignore for treatment planning, but matter if a measurement session's numbers look shifted from a prior one taken on a very different day.
- **Wavelength:** λ = c / f. A 100Hz wave is ~3.4m long; a 1kHz wave is ~34cm; a 31Hz wave (kanciapa's room fundamental, below) is ~11m — bigger than the room itself. This single fact explains why bass behaves so differently from treble in a small room: low frequencies don't "bounce around" like rays of light, they set up standing pressure fields that span the whole room.
- **Axial room mode frequencies** (parallel-wall standing waves, the strongest and most audible mode type): f_n = n·c / (2L), n = 1, 2, 3…, L = the distance between the parallel surfaces.

## Room modes — why kanciapa's corners matter so much

For kanciapa's 5.5m walls: f1 = 343/(2×5.5) ≈ **31.2Hz**, f2 ≈ 62.4Hz, f3 ≈ 93.5Hz. For the 3.5m height: f1 ≈ 49Hz, f2 ≈ 98Hz. Because length and width are both 5.5m, their axial modes land on **exactly the same frequencies** instead of spreading out — this is why `acoustics/PROJECT.md` calls out ~31Hz as a concentrated problem rather than a mild one. A non-square room spreads this energy across two different frequencies; kanciapa piles it into one.

A standing wave between two parallel walls has a **pressure maximum (antinode) at each wall** and a **pressure minimum (node) at the room's center**, for the fundamental — velocity is the mirror image (max at center, min at walls). This is the mechanism, not just an observation: air can't move *through* a rigid wall, so particle velocity must go to zero right at the boundary, which by the physics of a standing wave forces pressure to be maximal there instead.

**Corners are where axial modes across all three room dimensions overlap at their pressure maxima simultaneously** — length-mode, width-mode, and height-mode pressure antinodes all coincide at a corner. That's the actual reason bass traps go in corners: it's not an arbitrary convention, it's the one location in the room where you can address the most modal energy with one object. It also explains why kanciapa's 2-treated/2-untreated corner split is a legitimate A/B test — the treated corners should show measurably lower SPL at ~31Hz and its multiples than the untreated ones, which is exactly what the planned 4-corner REW measurement in `acoustics/PROJECT.md` is designed to reveal.

## Absorption: why bass needs thickness, treble doesn't

Standard porous absorption (foam, mineral wool, rockwool panels) works by **viscous friction**: air moving through the material's pores loses energy to heat. Since absorption needs air *velocity*, and velocity is near zero within a quarter-wavelength of a rigid boundary, a thin panel mounted flush on a wall is nearly useless against low frequencies — the air right at the wall isn't moving.

- A panel needs to be roughly **λ/4 thick (or spaced that far off the wall with an air gap) to absorb effectively down to a given frequency.** λ/4 at 31Hz is ~2.7m — obviously impractical as flat panel thickness, which is why low bass in a small room is not fixed by porous absorption alone.
- This is exactly why **corner bass traps use a different mechanism than flat wall panels**: they either rely on sheer bulk/density (large wedge or triangular traps with a lot of material intercepting the pressure-maximum region directly, even off-velocity-optimum) or use a **tuned resonator** (below) that's pressure-driven rather than velocity-driven and therefore works precisely where flat porous panels don't — right against the boundary.
- Mid/high frequencies (short wavelengths) are absorbed efficiently by thin material because λ/4 is centimeters, not meters — this is why kanciapa's existing flat panels are appropriate for taming flutter echo and general liveliness in the 500Hz+ range, but were never going to touch the 31Hz mode regardless of how many were added.

## Helmholtz resonance — tuned bass trapping

A Helmholtz resonator is a cavity of air (the "spring") connected to the outside through a narrow neck (a plug of air acting as the "mass") — push air into the neck, the cavity pressure pushes back, the system oscillates at a specific resonant frequency and dissipates energy strongly right at that frequency, largely independent of the λ/4-thickness limitation above (it's a mass-spring resonance, not a friction-through-thickness mechanism).

f = (c / 2π) × √(A / (V × L_eff))

where A = neck cross-section area, V = cavity volume, L_eff = neck length (plus an end-correction for the air just outside/inside the neck opening). Practical implication: if the 31Hz mode remains a problem after passive bass traps, a **tuned resonator (Helmholtz box or membrane/panel trap) sized specifically for ~31Hz** is the targeted next step — broadband porous trapping and narrowband tuned trapping solve different parts of the problem and are often combined, not substitutes for each other.

## Inverse-square law, near field, and far field

In free field, sound pressure level (SPL) falls **6dB per doubling of distance** from a point source. Indoors, this only holds close to the source (the **direct field**); beyond a certain distance (the **critical distance**, where direct and reverberant/reflected sound energy are equal), SPL flattens out because reflections dominate and no longer follow inverse-square. Critical distance shrinks as the room gets more reflective (less absorption) and grows as the source gets more directional.

Practical uses:
- **Close mic'ing** (cab mics, drum mics) deliberately stays well inside the direct field so the room's reflections/coloration barely register — this is why B906/SM57 positioned right at the cab grille capture "just the speaker," not the room.
- **REW measurement mic distance** trades off the same way: closer to the source captures more direct/speaker-specific response, farther out captures more of the room's actual in-use behavior (what a listener at the main spot really hears) — the 5-position plan in `acoustics/PROJECT.md` (4 corners + main spot) is implicitly sampling the reverberant field's spatial variation, which is the whole point of a room-mode survey.

## Directivity — why speakers and mics aren't the same at every frequency and angle

**Speaker directivity narrows as frequency rises** (a driver's radiating diaphragm becomes acoustically large relative to wavelength at high frequencies, which beams the sound). This is why toe-in/off-axis positioning of the Eris monitors matters far more for treble clarity than for bass — bass radiates close to omnidirectionally regardless of aim, so monitor *aim* mostly can't fix a bass problem (only monitor *position*, via the mechanisms above, can).

**Mic polar pattern determines what a mic is actually measuring:**
- **Cardioid/supercardioid** (B906, SM57) reject off-axis sound — appropriate for cab mic'ing, where the goal is isolating one source and rejecting room/bleed.
- **Omnidirectional** (ECM8000) picks up equally from all directions — this is *required*, not incidental, for room measurement: an omni mic captures the reverberant field impartially, which is what a directional mic would distort by favoring whatever it happens to point at. Never substitute a directional mic for room/REW measurement work.

## Phase and comb filtering — summing two mics on one source

When two mics pick up the same source from different distances, the sound arrives at each mic at a slightly different time. Summed together (to mono, or even close-panned stereo that collapses in mono playback), frequencies where the path-length difference equals a whole wavelength reinforce (constructive), and frequencies where it equals an odd multiple of a half-wavelength cancel (destructive) — a **comb filter**, a series of evenly-spaced notches and boosts across the spectrum, audible as a thin/hollow/swirly tone rather than a clean blend.

- Practical mitigation: keep both mics **equidistant from the source** where possible (equal path length → no comb filtering, since both signals stay in phase across all frequencies) — this is the reasoning behind kanciapa-notes.md's existing cab-mic guidance ("keep both mics equidistant from the cone").
- If mics can't be equidistant (e.g. one close, one further back for a room-tone blend), the standard rule of thumb is the **3:1 rule**: keep the second mic at least 3× as far from the first mic as the first mic is from the source — this pushes the resulting comb-filter notches high enough in frequency and shallow enough in depth to be much less audible.
- Same mechanism applies to overhead drum mic pairs, multiple room mics, or any future 2-mic cab experiment — always reason about it as "what's the path-length difference, and where does that put the first cancellation frequency" (first null at f = c / (2 × path difference)) rather than trial-and-error repositioning.

## Reading a REW measurement with this physics in mind

- **A broad peak at a frequency matching a calculated axial/tangential/oblique mode** (see formula above — try length, width, height and their sums/differences) → modal buildup, addressed by absorption/trapping at pressure-maximum locations (corners, or wherever the specific mode's antinode falls for that dimension), not by moving the listening position alone (though the listening position *can* be moved to a modal node to reduce audibility without fixing the room).
- **A narrow, sharp notch/null** → very often a comb-filter effect from a boundary reflection (Speaker Boundary Interference Response, SBIR — direct sound + a nearby-wall/floor/ceiling reflection combining, same comb-filter mechanism as the two-mic case above but with one path being source→boundary→ear instead of source→second-mic) — addressed by changing the speaker or listening position's distance from the offending boundary, since that changes the path-length difference and moves the notch frequency, whereas absorption changes its depth.
- **Different results at different mic positions for the same source** → expected and useful, not an error — standing waves mean SPL genuinely varies by position; this is precisely what the 4-corner + main-spot measurement plan is designed to map.

## When advising

- Check `~/projects/music/kanciapa/acoustics/PROJECT.md` first for what's already measured/decided in this specific room — don't re-derive from scratch what's already been established on-site.
- When a question is about *this* room, compute the actual mode frequencies for 5.5×5.5×3.5m rather than giving generic "bass builds up in corners" advice — the specific numbers (31.2Hz, 62.4Hz, 49Hz, etc.) are what make a recommendation actionable and checkable against a REW graph.
- Keep the electrical and the acoustic domains separate: hum/buzz/ground loops/gain staging are `av-signal-chain` territory (electrical), while anything about how the sound wave itself propagates, reflects, or sums in air is this skill's territory. A "why does it sound bad" complaint can turn out to be either — ask or check both if the cause isn't obvious from context.
- Prefer measurement over guessing when REW/the mic is available — physics tells you *what to expect and why*, but the room's actual behavior (real furniture, real wall construction, real speaker) should be confirmed against a sweep before committing to a treatment purchase.
