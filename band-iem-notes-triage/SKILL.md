---
name: band-iem-notes-triage
description: Use when Dawid asks to read, review, triage or process his BandIEM feedback notes (the Notes button in the app), says "prepare fixes from my notes", or STARTS the BandIEM / kanciapa project (then check for new notes first). Review = a plain list of fixes, ideas, questions and hardware items with screenshots, no questioning round. Never implements before Dawid says which items to do.
---

# BandIEM notes review (list mode)

Repo: `~/projects/music/kanciapa/band-iem`. Description of the tooling: `docs/notes-triage/README.md`.

## When Dawid starts the project (loads kanciapa / BandIEM / bunkier work)

1. Run `cd ~/projects/music/kanciapa/band-iem && node scripts/notes-triage/fetch-notes.mjs --source nest-dev --new` (read-only; if nest's dev app is down, say so in one line and carry on).
2. No new notes: say nothing about notes (or one short line if `docs/notes-triage/LIST.md` still has items he has not answered).
3. New notes: tell him first thing, before other status: how many, then do the review below and show the list.

## Review = make a list, no questions

1. **Fetch** the open notes: `node scripts/notes-triage/fetch-notes.mjs --source nest-dev` (saves the brief and downloads screenshots to `docs/notes-triage/inbox/`, which is git-ignored). `bunkier-dev` only when Dawid says bunkier is on. If fetching fails, report the message and stop.
2. **Look at every screenshot** (Read the image file) together with the note text, mode (the screen) and context.
3. **Light investigation:** `git grep` the screen and the words of the note so each item has a one-line diagnosis. Do not spend long; this is a list, not a design.
4. **Write `docs/notes-triage/LIST.md`** (English, quote the original wording), replacing the previous list, with these sections and nothing else:
   - **Fixes** (bugs, tuning): key, quote, one-line cause, proposed fix, effort (S/M/L), screenshot path if any.
   - **Ideas** (features, wishes): same fields.
   - **Questions** (unclear notes): what is unclear, one question each.
   - **Hardware / rig:** things that are not app code.
   Keep a dated copy only when asked. Commit the list on a short branch merged INTO `main`.
5. **Mark them listed:** `node scripts/notes-triage/fetch-notes.mjs --source nest-dev --mark-seen` and commit `docs/notes-triage/seen.json` with the list.
6. **Tell Dawid** the counts per section and the list itself in a compact table. Do not ask the questions one by one; they sit in the list for him to answer when he wants.
7. **Implement only on his word** ("do 2 and 5"): test first, short branch merged INTO `main`, say exactly which branch went into which. Dawid ticks notes done in the app; never mark or delete a note yourself.

## Rules

- Read-only on notes sources. Machines that are off stay untouched; wait for Dawid to say they are on.
- Do not mention or touch the protected release branch.
- Keep replies short.
