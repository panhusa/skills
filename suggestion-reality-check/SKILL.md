---
name: suggestion-reality-check
description: Use when generating lists of topic suggestions, learning ideas, tool recommendations, or project proposals for a specific user. Invoke before presenting any list of N items to avoid forced/na-siłę suggestions that don't fit the user's actual setup.
---

# Suggestion Reality Check

Before presenting a list of suggestions, each item must pass a concrete ownership test — not a vibes test.

## The Test (per item)

Ask: **"Am I recommending this because it genuinely fits or interests this person — or just to fill a slot?"**

Three valid reasons to include something:
1. **Extends what they have** — bridges to existing tools/workflow
2. **Fits who they are** — coheres with their style, personality, or stated interests
3. **Genuinely cool and unknown to them** — something they likely haven't heard of that could spark real interest

New topics the user hasn't encountered are fine — and sometimes the best pick.
Non-technical domains (psychology, geography, science, history, philosophy) are equally valid — don't default to tech.

| Passes | Fails |
|--------|-------|
| Extends existing workflow | Vague category leap ("into music → maybe theory?") |
| Coheres with personality/interests | Added just to reach a target number |
| Genuinely novel + potentially exciting | "Could be useful someday" with no scenario |
| You can say *why this person specifically* | You can only say *why people in general* |

## Red Flags (remove the item)

- You can't explain why *this person* vs. anyone with a computer
- Requires a blocker (expensive gear, platform lock-in) with no stated interest
- You're padding to hit a number — if 6 solid ideas exist, present 6

## Baseline Failure (real example)

Session 2026-06-19: generated 15 topics, 6 failed the test:
- OSC → no Ableton OSC plugin, no concrete bridge problem
- Sonic Pi → user does DAW production, not live coding performances  
- ESP32 BLE scanner → no stated problem it solves
- GLSL shaders → photography ≠ visual programming
- Bash `<()` → 30-second tip, not a learning session
- Inkscape trace bitmap → no stated vector/design workflow

Result after applying this skill: 9 solid topics remained.

## Workflow

1. Generate full list internally
2. For each item: apply the ownership test
3. Remove any that fail or raise red flags
4. Present only survivors — do NOT pad to reach a target number
5. If fewer than 3 survive, say so honestly
