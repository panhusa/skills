---
name: save-feedback
description: Save behavioral feedback to persistent memory so Claude remembers how to work with this user in future sessions. Invoke this skill whenever the user says "zapisz feedback", "save feedback", "zapamiętaj że", "dodaj do memory", "zapisz zasadę", "dodaj to do memory", or similar. Also invoke proactively — without being asked — when the user corrects Claude's approach ("nie rób X", "stop doing Y") or confirms something worked well ("dokładnie tak", "tak właśnie"). Behavioral corrections are the most important things to capture. If in doubt, save it.
---

## What this skill does

Writes a structured feedback file to `~/.claude/projects/-home-hushus/memory/feedback/` and updates the MEMORY.md index. The goal is that the next Claude session picks up this rule immediately, without the user having to repeat themselves.

## Step 1 — Extract the rule

From the conversation, identify:
- **The rule**: what Claude should or shouldn't do
- **Why it matters**: what went wrong or what the user values
- **When it applies**: the context

If any of these is genuinely unclear, ask one short question. Most of the time you can infer enough from context.

## Step 2 — Check for an existing file

```bash
ls ~/.claude/projects/-home-hushus/memory/feedback/
```

If a file already covers this topic, update it instead of creating a duplicate. A focused updated file is better than two overlapping ones.

## Step 3 — Write or update the file

Filename: `feedback_<topic>.md` — lowercase, underscores, descriptive.

Use this exact frontmatter format (matches existing files):

```markdown
---
name: feedback-<topic>
description: "<one sentence — specific enough to decide relevance in future sessions>"
metadata:
  node_type: memory
  type: feedback
---

<The rule, stated directly in 1-3 sentences.>

**Why:** <The reason — what happened or what the user values.>

**How to apply:** <When this kicks in and what to do.>
```

Keep the body under 10 lines. The description field is what future sessions see first — make it specific and actionable, not vague.

**Example:**

```markdown
---
name: feedback-bugfix-focus
description: "Bug fixes: change only what's needed, no opportunistic cleanup"
metadata:
  node_type: memory
  type: feedback
---

When asked to fix a bug, make only the minimal change needed. Do not clean up surrounding code, rename variables, or refactor while at it.

**Why:** Unasked-for changes muddy the diff and introduce unrequested risk.

**How to apply:** Any scoped fix = surgical change only. Note cleanup opportunities separately but don't apply them.
```

## Step 4 — Update MEMORY.md

Add one line under `## Feedback` in `~/.claude/projects/-home-hushus/memory/MEMORY.md`:

```
- [feedback/<filename>.md](feedback/<filename>.md) — <hook under 100 chars>
```

If you updated an existing file, update its description in MEMORY.md if the old one no longer fits.

## Step 5 — Report

One sentence: what was saved or updated and where. Nothing else.
