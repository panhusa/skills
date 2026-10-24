---
name: spec-coherence-review
description: Audits a set of related planning/spec/plan Markdown documents (design docs, PROJECT.md-style decision logs, implementation plans) for internal consistency with each other. Use this whenever the user asks to review, audit, or check whether a project "holds together," "makes sense," or is "spójny"/"trzyma się" — especially right after editing one spec/plan/decision doc that other docs in the same project reference or depend on. Also trigger proactively when you notice you're about to hand off a freshly-edited spec or plan to implementation and haven't checked it against its sibling documents yet. This is strictly for prose/Markdown planning documents, before or without any code existing — it does NOT review code diffs (use /code-review for that) and does NOT review a single document in isolation (use normal reading/self-review for that; this skill's value is specifically in cross-document consistency).
---

# Spec Coherence Review

Multi-document specs rot silently. You edit the design doc to add a feature,
but the decision log (`PROJECT.md`-style) still describes the old behavior.
You split one planned task into two while writing an implementation plan,
but three other tasks still say "see Task 14" for something that's now
Task 16. Nobody notices until someone reads the stale reference and either
gets confused or — worse — builds against it. This skill's whole job is
catching that class of bug before a human or another agent trips on it.

## Why this needs its own pass, not just "read it once"

Writing and reviewing are different modes. When you write or edit a
document, you're focused on the section in front of you — the edit is
locally correct. Staleness lives in the *other* documents that referenced
what you just changed, which you're not looking at while editing. Catching
that requires deliberately re-reading everything as a single reader would,
after the fact, specifically hunting for disagreements — not re-reading any
one document for its own sake.

## Step 1: Identify the document set

If the user names the documents, use those. If they don't, infer the set:
start from whatever was most recently edited in this conversation or
session, then include anything that document explicitly references (a
`PROJECT.md` pointing at a `plans/*.md` spec, a spec pointing at an
implementation plan, a plan referencing the spec it implements) plus
anything that references *it* (grep the project directory for the edited
file's name or its distinctive terms). When genuinely unsure which
documents are "related," ask — better one clarifying question than
auditing the wrong set.

## Step 2: Read every document in the set fully

Not excerpts, not the sections that seem relevant — the whole file. Stale
references and contradictions hide in sections nobody expected to touch.

## Step 3: Build a checklist of claims that must agree

While reading, extract the load-bearing facts each document makes that
another document in the set also depends on or restates:

- **Terminology and decisions.** A concept renamed or redefined in one
  place (e.g. "6 fixed profiles" becoming "any count via a preset system")
  — does every other document that used the old framing get updated, or
  does it silently still describe the old world?
- **Numbering and cross-references.** Task/section/step numbers, "see
  Task N," "as described in section X" — do these still point at the
  right target after edits, splits, or reordering?
- **Scope boundaries.** What a spec promises vs. what a plan actually
  implements. A plan is allowed to defer scope — but only if it says so.
  The bug is a *silent* gap: a spec requirement with no corresponding task
  and no mention in an "out of scope" / "deferred" list.
- **Data shapes and interfaces.** If multiple documents describe the same
  type, schema, or function signature (e.g. one doc's schema table vs.
  another doc's field list), do they actually match field-for-field?

## Step 4: Cross-check, don't just summarize

For each item on the checklist, actively look for the disagreement — don't
just confirm the happy path. A useful trick: for every "this is now X, not
the old Y" decision, grep the whole document set for the old term/number to
make sure it didn't survive somewhere the edit missed.

## Step 5: Report findings

Report format — concrete and skimmable, not a wall of prose:

```markdown
**Real issues (fix recommended):**
1. <one-line claim> — `path/to/file.md` §<section or line range>
   contradicts `path/to/other.md` §<section> — <what's actually wrong>

**Looks fine (false alarms avoided):**
- <thing that looked suspicious at a glance> — actually consistent
  because <reason>, checked against <other doc>.
```

Two things matter here:

1. **Distinguish real issues from false alarms explicitly.** Don't just
   list suspicions — say which ones you checked and ruled out, and why.
   This is what makes the report trustworthy instead of a pile of maybes
   the reader has to re-verify themselves.
2. **If everything actually holds together, say so plainly and stop.**
   Manufacturing findings to look thorough is worse than useless — it
   trains the reader to stop trusting the report. A clean bill of health
   is a valid, complete result.

Always cite file + section/line so the reader (human or another agent) can
jump straight to the spot, not re-derive it.
