---
name: product-concept-paper
description: Use when asked to write a Product Concept Paper, PRD, Product Proposal, or Product Requirements Document for a new product, feature, or internal initiative. Triggers when the user wants to pitch an idea, align stakeholders, or document a project blueprint before building begins.
---

# Product Concept Paper

## Overview

A 2–5 page concise blueprint to pitch a product idea and align stakeholders **before** building begins. Also called: Product Concept Paper, Product Proposal, PRD.

**Core principle:** Every section answers one stakeholder question. No fluff. Decisions over descriptions.

## Required Sections

| # | Section | Purpose | Stakeholder Question |
|---|---------|---------|---------------------|
| 1 | **Executive Summary** | Problem + solution in 3 bullets max | "What is this?" |
| 2 | **Problem Statement** | Pain points, impact, who is affected | "Why does this matter?" |
| 3 | **Proposed Solution** | What we're building, not how | "What are we doing?" |
| 4 | **Target Users** | Primary + secondary personas | "Who is it for?" |
| 5 | **Key Features / Scope** | In scope / out of scope table | "What exactly?" |
| 6 | **Success Metrics** | Measurable KPIs with targets | "How do we know it worked?" |
| 7 | **Financials** | Cost, savings, alternatives | "What does it cost?" |
| 8 | **Roadmap** | Phases with status and outcomes | "When?" |
| 9 | **Risks & Mitigations** | Top risks with likelihood/impact | "What could go wrong?" |
| 10 | **The Ask** | Explicit requests: approval, sponsorship, budget | "What do you need from me?" |

## Format Rules

- **Length:** 2–5 pages max (or equivalent Markdown)
- **Tone:** Direct, executive-friendly — no jargon without definitions
- **Tables over prose** for: costs, risks, features, KPIs, roadmap
- **Bold the key data point** in each table cell when possible
- **No passive voice** in problem/solution sections
- Speaker notes / internal notes → HTML comments `<!-- ... -->`
- Open items → explicit `[ ]` checklist at the end, not buried in text

## Document Header

```markdown
# [Product Name] — Product Concept Paper

| Field       | Value              |
|-------------|--------------------|
| Author      | [Name]             |
| Version     | [v1.0]             |
| Date        | [Month Year]       |
| Status      | [Draft / Review / Approved] |
| Audience    | [Role(s)]          |
```

## Problem Statement Template

```markdown
## Problem

| Problem | Business Impact |
|---------|----------------|
| [Specific gap] | [Measurable or qualitative consequence] |
```

End with: *"[One sentence framing the risk of doing nothing.]"*

## The Ask Section

Always list explicit asks — one per bullet, action-oriented:

```markdown
## What We Need

- **[Action]** — [from whom] — [why / by when]
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Solution-first (no problem section) | Always lead with pain, then solution |
| Vague scope ("will do X eventually") | In-scope / out-of-scope table |
| No KPIs | Every outcome must be measurable |
| Buried ask | Explicit final section: "What I need from you" |
| Too long | Cut anything that doesn't answer a stakeholder question |
| Open items buried in prose | Move to `## Open Items` checklist at end |
