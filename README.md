# gsd:design-ui

A taste-first AI design skill for [GSD (Get Shit Done)](https://github.com/glittercowboy/get-shit-done) — Claude Code's AI workflow framework.

Turns a screenshot or design brief into production-ready UI code through a structured 6-stage pipeline where every prompt is pre-loaded with context before generation begins.

---

## Requirements

- [GSD (Get Shit Done)](https://github.com/glittercowboy/get-shit-done) installed via `npx get-shit-done-cc`
- Claude Code

## Install

Copy `design-ui.md` into your project's GSD commands folder:

```bash
cp design-ui.md .claude/commands/gsd/design-ui.md
```

Then use it in Claude Code:

```
/gsd:design-ui Replicate this Figma dashboard --folder frontend/src
```

---

## How It Works

Most AI-generated UI is slop because the model generates cold — no references, no vocabulary, no constraints injected before the first token.

This skill fixes that with **6 stages** and a **3-pass zoom-in method**:

```
Stage 1: Intake      — brief, references, constraints → design-context.md
Stage 2: Research    — analyze references, domain patterns → design-research.md
Stage 3: Plan        — tokens, components, wireframes → design-system.md
Stage 4: Verify Plan — critique before any code is written
Stage 5: Execute     — Pass 1 (50%) → Pass 2 (99%) → Pass 3 (100%)
Stage 6: Verify      — senior designer critique, sign-off checklist
```

Every stage pre-loads the output of the previous stage. Nothing runs cold.

---

## Usage

```bash
# Full workflow
/gsd:design-ui <description> --folder <path>

# Skip research (when you already have references analysed)
/gsd:design-ui <description> --folder <path> --skip-research

# Skip verification gates (faster, less rigorous)
/gsd:design-ui <description> --folder <path> --skip-verify

# Resume from Pass 2 (self-review)
/gsd:design-ui --folder <path> --pass2

# Resume from Pass 3 (micro polish)
/gsd:design-ui --folder <path> --pass3
```

---

## Output Files

After a full run, `--folder` will contain:

| File | Purpose |
|------|---------|
| `design-context.md` | Brief, visual references, vocabulary, constraints |
| `design-research.md` | Reference analysis, domain patterns, component inventory |
| `design-system.md` | Token system, component specs, layout wireframes |
| `design-verify-plan.md` | Pre-execution critique results |
| `design-pass1-notes.md` | Pass 1 gaps and decisions |
| `design-pass2-notes.md` | Self-review fix list |
| `design-pass3-notes.md` | Micro polish log |
| `design-verification.md` | Final critique + sign-off checklist |
| *(generated code)* | Your actual UI files |

---

## The 3-Pass Method

| Pass | Target | What happens |
|------|--------|-------------|
| Pass 1 | ~50% | Full context dump — structure + rough aesthetic |
| Pass 2 | ~99% | AI self-review — catches ~70% of its own mistakes |
| Pass 3 | 100% | Micro polish — pixel-level precision with exact values |

Most AI design tools stop at Pass 1. The quality gap lives in 2 and 3.

---

## Works With Any Stack

The skill adapts to whatever constraints you provide in the brief:
- Next.js + Tailwind
- React + CSS modules
- Vue + custom properties
- Plain HTML/CSS

---

## License

MIT
