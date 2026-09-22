---
name: self-benchmark
description: "Produces an honest, evidence-grounded self-assessment of Dawid's skills, strengths/weaknesses, and thinking patterns, benchmarked against real published frameworks (not invented numbers). Use when he asks to be rated, evaluated, benchmarked, or wants honest feedback on his skills or way of thinking — e.g. 'rate me', 'how good am I at X', 'be honest with me', 'what are my weak/strong sides'."
---

# Self-benchmark

Dawid explicitly wants unflinching honesty here — he called this kind of feedback "a gift," said he can take it or leave it, and asked not to be held back. Treat that as a standing instruction for this skill, not a one-off mood. Still: ground every claim in real evidence, never in vibes, and never fabricate false precision.

## Hard rules

1. **No fake precision.** Never give a decimal-point score (e.g. "7.34/10") for a subjective judgment — say so if asked. Whole or half-point ratings are fine, always with reasoning attached.
2. **Evidence only.** Every rating must cite something real — a file, a project, a specific session event. If there's no evidence for a domain, say "unrated, not enough evidence" rather than guessing.
3. **Distinguish his hands-on skill from AI-assisted output.** For anything built substantially through Claude Code, separate "technical architecture/product judgment" (his) from "hand-written code fluency" (often not his) — don't credit him for code he didn't write.
4. **Research each domain, don't rely on priors.** Use WebSearch for real skill-progression frameworks/benchmarks per domain before rating (e.g. DevOps competency ladders, music production gear tiers, photography skill levels). Cite sources.
5. **"Way of thinking" section is required, not optional**, when he asks for a fuller assessment (not just a skills table): decision-making style, completion patterns, communication style, blind spots — grounded in observed behavior across memory/projects, not personality-test guessing. Ground claims like "impulsive decision style" or "project completion pattern" in real published research (Zeigarnik effect, fast-vs-impulsive decision literature, polymath/generalist trade-offs, etc.) the same way technical domains get benchmarked.
6. **Mental-health-adjacent observations** (isolation, follow-through on wellbeing tasks, stress patterns) may come up naturally from his own logged data (e.g. `reminders.md`). If so: state the observation factually and briefly, pair it with validation of the real external factor behind it (e.g. distributed async team = genuine isolation), never diagnose, never moralize or turn it into a lecture. This is an observation from his own data, not a clinical opinion.
7. **Ask when evidence is thin**, rather than filling gaps with assumption — e.g. "I have zero photography output to judge, only gear choices — is there a portfolio somewhere I haven't seen?"
8. **This system sees a curated slice of his life, not the whole picture — by his own deliberate choice, confirmed 2026-09-23.** He doesn't confide everything here and deliberately keeps some things out of these sessions while he decides when the time is right for them. Absence of visible activity in memory/projects is NOT evidence of absence of real-world progress. State completion-pattern or behavioral observations as "this is what's visible here," never as "this isn't happening" — and say so explicitly in the output, not just internally.

## Workflow

```
Progress:
- [ ] Re-read prior self-assessment(s) in notes/self-assessments/ (if any) for trend comparison
- [ ] Gather evidence: user memory files, active_project.md, PROJECTS.md, reminders.md, relevant project repos
- [ ] Identify domains to assess (technical + "way of thinking") based on what's actually evidenced
- [ ] WebSearch a real benchmark/framework per technical domain
- [ ] Draft ratings + reasoning, flag anything unrated for lack of evidence
- [ ] Draft "way of thinking" section: strengths, weaknesses, blind spots — cite specific evidence for each
- [ ] Ask any clarifying questions needed before finalizing
- [ ] Write dated report to notes/self-assessments/YYYY-MM-DD-self-assessment.md
- [ ] Present a tight summary in chat, point to the file for full detail
- [ ] Do NOT commit/push the report to git without explicit confirmation — this content is more sensitive than typical project docs
```

## Output location

`~/projects/notes/self-assessments/YYYY-MM-DD-self-assessment.md` — one file per run. If a prior file exists, the new report should explicitly note what changed (trend), not just repeat a fresh snapshot.
