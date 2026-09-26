# Skills — ~/.claude/skills/

Single repo for all Claude Code skills.
GitHub: github.com/panhusa/skills

---

## Custom (Dawid)

| Skill | Purpose |
|---|---|
| `ac-setup-generator` | AC racing setup .ini + .md generator |
| `av-signal-chain` | Pedalboards, patchbays, FX loops, gain staging |
| `blender-3d-print` | Blender → 3D print workflow |
| `brainstorming` | Before any creative/feature work |
| `clean-code` | Code quality review |
| `git-commit-helper` | Commit messages |
| `kanciapa` | Orientation for the kanciapa jam/recording room project |
| `kanciapa-mx5-scenes` | HeadRush MX5 rig/scene setup for the kanciapa chain |
| `laptop-ubuntu-kanciapa` | G5 EliteBook / REAPER / kanciapa MCP (stale, see its description) |
| `office-files` | .docx/.xlsx/.pptx via python-docx, openpyxl, python-pptx |
| `product-concept-paper` | Product concept docs |
| `react-best-practices` | React patterns |
| `save-feedback` | Persist behavioral feedback to memory |
| `self-benchmark` | Evidence-grounded self-assessment |
| `senior-architect` | Architecture decisions |
| `senior-prompt-engineer` | Prompt engineering |
| `sound-physics` | Acoustics grounded in physics (room modes, mics, treatment) |
| `suggestion-reality-check` | Filter forced/irrelevant suggestions |
| `system-analyzer` | Homelab infrastructure audit |
| `ui-ux-pro-max` | UI/UX review + design-system search (`scripts/search.py`) |
| `vulnerability-scanner` | Security scan |

## To build (plan: notes/skills-plan.md in ~/projects)
- `kanciapa-start` — REAPER session setup
- `life-os` — voice/habit/expense logging
- `projects-status` — daily orientation
- `arduino-sketch` — ESP32/Nano boilerplate
- `simracing-session` — AC server start

---

## Local copies of official/marketplace skills

`doc-coauthoring`, `frontend-design`, `mcp-builder`, `pdf`, `skill-creator`,
`theme-factory`, `webapp-testing`

Everything else official (`docx`, `pptx`, `xlsx`, `claude-api`, `canvas-design`,
`algorithmic-art`, `brand-guidelines`, `internal-comms`, `slack-gif-creator`,
`web-artifacts-builder`, `copywriting`) comes from the plugin marketplace, not
this repo.

## Validation

```bash
for d in */; do python3 skill-creator/scripts/quick_validate.py "$d"; done
```
