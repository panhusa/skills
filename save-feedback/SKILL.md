---
name: save-feedback
description: Save behavioral feedback to persistent memory so Claude remembers how to work with this user in future sessions. Invoke this skill whenever the user says "zapisz feedback", "save feedback", "zapamiętaj że", "dodaj do memory", "zapisz zasadę", "dodaj to do memory", or similar. Also invoke proactively — without being asked — when the user corrects Claude's approach ("nie rób X", "stop doing Y") or confirms something worked well ("dokładnie tak", "tak właśnie"). Behavioral corrections are the most important things to capture. If in doubt, save it.
---

## What this skill does

Adds a rule to one of the themed feedback files in `~/.claude/projects/-home-hushus/memory/feedback/` (or a tool fact to `reference/tool_gotchas.md`). The goal is that the next Claude session picks up the rule immediately, without the user repeating it, and that the feedback folder stays small (one file per theme, not one file per rule; consolidated 2026-10-07).

## Step 1 — Extract the rule

From the conversation, identify:
- **The rule**: what Claude should or shouldn't do
- **Why it matters**: what went wrong or what the user values (include the date)
- **When it applies**: the context

If any of these is genuinely unclear, ask one short question. Most of the time you can infer enough from context.

## Step 2 — Pick the themed file

| Rule is about | File |
|---|---|
| asking vs acting, confirmation, data safety, credentials, deletions, autonomy grants, cleanup | `feedback/rules_action_and_safety.md` |
| tone, answer format, language, token use, understanding the ask, self-checking | `feedback/rules_communication_and_style.md` |
| keeping docs current, evidence vs documentation, research habits, checking existing state | `feedback/rules_docs_and_truth.md` |
| git and branches, backlog and session scripts, writing in chapters, subagents, secrets and keys, work vs personal | `feedback/rules_git_and_workflow.md` |
| band-iem, bunkier, VB-Matrix, live rig, stable and dev trees | `feedback/rules_band_iem.md` |
| buying hardware, MVP, physical-task planning | `feedback/rules_hardware_and_physical.md` |
| a fact about a tool or system (how to launch, an API call, a dead end), not about how Claude behaves | `reference/tool_gotchas.md` |

Read the target file first (`grep -n '^## ' <file>`) and check the rule is not already there; if it is, sharpen the existing bullet instead of adding a second one.

## Step 3 — Append to the file

Add the rule as a bullet (or extend an existing bullet) under the fitting `##` section, in this shape:

`- **<short rule name>** (<YYYY-MM-DD>): <the rule in 1-2 sentences>. Why: <what happened or what the user values>.`

Keep it to 2-4 lines. Write in English. Do not touch the frontmatter except when the file's `description` no longer fits the new content.

Only if NO theme fits, create `feedback/rules_<theme>.md` with this frontmatter and add one index line (Step 4):

```markdown
---
name: rules-<theme>
description: "<one sentence listing what the file covers>"
metadata:
  type: feedback
---
```

Never create a one-rule file named `feedback_<topic>.md` again.

## Step 4 — Index (only when a new file was created)

Themed files are already in `MEMORY.md` under `## Feedback`. Add a line only for a brand-new file:

```
- [feedback/<filename>.md](feedback/<filename>.md) — <hook under 100 chars>
```

## Step 5 — Report

One sentence: what was saved, in which file. Nothing else.
