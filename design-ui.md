---
name: gsd:design-ui
description: Taste-first AI design workflow — context → research → plan → verify → execute (3 passes) → verify. Embeds the full design learning so every prompt is pre-loaded with the right project context, vocabulary, and references before processing.
argument-hint: "<description> --folder <path> [--skip-research] [--skip-verify] [--pass2] [--pass3]"
---

<!--
  HOW THIS SKILL WORKS
  ─────────────────────────────────────────────────────────────────
  This skill embeds the full "taste-first AI design" methodology.

  The key principle: every prompt that goes to any AI model is
  pre-loaded with the correct context BEFORE processing begins.

  Concretely:
  - Before generating a design system → inject design-context.md + design-research.md
  - Before executing code → inject design-system.md + design-context.md
  - Before running critique → inject design-system.md + the actual code
  - Before micro polish → inject pass 2 notes + design-system.md

  Nothing is generated cold. Context always precedes generation.
  This is the single biggest difference between AI slop and designed output.
  ─────────────────────────────────────────────────────────────────
-->


<!-- ============================================================
     EMBEDDED DESIGN KNOWLEDGE BASE
     (Read this section first — it governs every prompt generated)
     ============================================================ -->
<design_knowledge>

## Core Principle: Taste, Not Prompts

AI excels at patterns it has seen. Your job is knowing which patterns
are worth copying — and when to refuse what the AI produces.

The designers excelling with AI in 2026 are not the ones with the
longest prompts or newest tools. They are the ones who:
- Study hundreds of interfaces and learn to name what they see
- Feed curated visual references into every session (AI is blind without them)
- Run systematic passes rather than expecting perfection in one shot
- Know when to override the machine — and have the vocabulary to say why

**What AI cannot do:**
- Tell you if your user story is wrong
- Know when "good enough" is actually good enough
- Have taste — it has pattern recognition, which is not the same thing
- Maintain coherence without a context file
- Make the call when two valid options conflict

**What this skill does:**
It automates the scaffolding so you can focus on judgment.

---

## The 3-Pass Zoom-In Method

Never expect AI to nail a design in one shot.
Guide it through the same progression a professional uses:
low-fidelity → high-fidelity → polish.

**Pass 1 (50%):** Full context dump → structure + rough aesthetic
**Pass 2 (99%):** Self-review → AI catches ~70% of its own mistakes
**Pass 3 (100%):** Micro polish → pixel-level precision with exact values

The gap between AI slop and designed output lives in passes 2 and 3.
Most people only do pass 1.

---

## The Video→Code Shortcut

For any site you admire: record a screen capture. Use this prompt
with the current best front-end model:

```
Analyze this website video and recreate the HTML/CSS/JS:

LAYOUT & STRUCTURE:
- Maintain exact visual hierarchy (header, sections, footer)
- Preserve spacing relationships (margins, padding, gaps)
- Replicate the grid system and responsive behavior

TYPOGRAPHY:
- Match font sizes, weights, and line heights
- Preserve text hierarchy (H1, H2, body, captions)
- Maintain letter spacing and text alignment

COMPONENTS:
- Recreate navigation structure and behavior
- Build cards, buttons, forms with matching styles
- Include hover states and interactive elements

ANIMATIONS & INTERACTIONS:
- Capture scroll-triggered animations and timing
- Replicate transition speeds and easing functions
- Include sticky elements, parallax, fade effects

OUTPUT:
- Clean, semantic HTML structure
- Organized CSS with clear class naming
- Functional JavaScript for interactions
- Component-based architecture where possible
```

Captures ~80% of the aesthetic immediately. Use as a reference
bootstrap, then refine through the 3-pass system.

---

## Design Vocabulary (Precision Language)

You cannot prompt what you cannot name.
"Make it prettier" gives AI nothing. These terms give it everything.

**Typography:**
- **Hierarchy** — H1→H6→body→captions (reading order, not just size)
- **Kerning** — space between two specific letters
- **Leading** — vertical line spacing (line-height in CSS)
- **Weight** — thin(100)/light(300)/regular(400)/medium(500)/semibold(600)/bold(700)/black(900)
- **Tracking** — letter-spacing across a word or block

**Layout:**
- **White space** — breathing room — the negative space that creates focus
- **Proximity** — visual grouping of related elements
- **Contrast ratio** — text-to-background luminance (WCAG AA: 4.5:1 normal, 3:1 large)
- **Visual rhythm** — consistent spacing intervals that create flow
- **Baseline grid** — invisible horizontal lines that type sits on

**Color:**
- **Primary** — brand colors (max 2), used sparingly
- **Accent** — CTA and highlight color — intentional, not everywhere
- **Semantic** — error(red) / success(green) / warning(amber) — reserved for meaning only
- **Neutral** — gray scale for text, borders, backgrounds
- **Token** — a named reference (--color-accent) not a hardcoded hex

**Components:**
- **State** — default / hover / focus / active / disabled / selected / error
- **Variant** — different visual style of same component (filled/outline/ghost)
- **Size** — scale dimension (sm/md/lg) of a component

**Precision examples that work:**
- ✓ "Reduce the leading on H2s to 1.4"
- ✓ "Increase white space between sections from 32px to 48px"
- ✓ "Add 1px border in border-subtle token on card default state"
- ✗ "Make this prettier"
- ✗ "It feels off"
- ✗ "Make it more modern"

---

## Reference Sources for Taste Building

Before touching any AI tool: 20–30 minutes gathering screenshots of
interfaces that solve the same problem you're working on.

- **Awwwards** — cutting-edge web design
- **Dribbble** — UI patterns at component level
- **Pinterest AI search** — upload a screenshot, find similar
- **Linear, Stripe, Vercel, Supabase, Arc** — consistently high craft

For each reference, don't just save it. Name what works:
- Is it the hierarchy?
- The spacing restraint?
- The color discipline?
- The typographic rhythm?

That naming is what feeds precise AI prompts.

</design_knowledge>


<!-- ============================================================
     CONTEXT LOADING PROTOCOL
     ============================================================ -->
<context_loading_protocol>

## Rule: No Prompt Runs Cold

Before EVERY generation step in this workflow, load the following
into the prompt in this order:

**At Stage 3 (Plan):**
```
[design-context.md full content]
[design-research.md full content]
```

**At Stage 5 Pass 1 (Execute):**
```
[design-context.md full content]
[design-system.md — tokens + components sections]
[design-system.md — layout wireframes section]
```

**At Stage 5 Pass 2 (Self-review):**
```
[design-system.md full content]
[current code being reviewed]
[design-pass1-notes.md if exists]
```

**At Stage 5 Pass 3 (Micro polish):**
```
[design-system.md tokens section]
[design-pass2-notes.md — the numbered fix list]
[specific component/screen code]
```

**At Stage 6 (Verify):**
```
[design-system.md full content]
[all generated code or description of screens]
[design-pass3-notes.md]
```

</context_loading_protocol>


<!-- ============================================================
     STAGE 1 — INTAKE
     ============================================================ -->
<stage id="intake">

## 1. Parse Arguments

Extract from $ARGUMENTS:
- Design goal / description (everything before any flag)
- `--folder PATH` — target output directory (required)
- `--skip-research` — skip Stage 2
- `--skip-verify` — skip Stages 4 and 6
- `--pass2` — resume from Pass 2 (self-review)
- `--pass3` — resume from Pass 3 (micro polish)

If `--pass2` is set: skip to Stage 5 Pass 2.
If `--pass3` is set: skip to Stage 5 Pass 3.
If `--folder` is missing: ask for it before proceeding.

## 2. Display Intake Header

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► DESIGN INTAKE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 3. Ask the Brief Questions

**Always ask (block if missing):**
1. What exactly are you building? (one sentence — product type + core user action)
2. Who is the target user? (role + key behavior — the more specific the better)
3. Confirm the target folder path.

**Ask if the brief is thin (optional but powerful):**
4. Any visual references? (URLs, product names like "Linear + Arc", screenshot paths)
5. What should this NOT feel like? (prevents generic AI output)
6. Hard technical constraints? (Next.js + shadcn/ui? Dark mode only? Mobile-first?)

Extract what you can from $ARGUMENTS first.
Only ask for what's genuinely missing.

## 4. Detect Existing Design Context

Check if `$TARGET_FOLDER/design-context.md` exists.

If found: read it in full, display summary, ask:
> "Found existing design-context.md. Use as-is, update with new info, or start fresh?"

## 5. Write design-context.md

Write to `$TARGET_FOLDER/design-context.md`:

```markdown
# Design Context

**Created:** [date]
**Project:** [name or description]

<brief>
## Brief
**What:** [one-sentence description]
**Who:** [target user — role + key behavior]
**Core action:** [what the user primarily does in this UI]
</brief>

<references>
## Visual References
[List each reference with WHY it's relevant]
- [Reference 1] — [what specifically to take from it]

**Anti-references (what this should NOT feel like):**
- [Anti-reference if provided]
</references>

<constraints>
## Constraints
**Tech stack:** [framework, component library]
**Platform:** [web / mobile / both]
**Color mode:** [light / dark / both]
**Accessibility target:** WCAG AA minimum
**Other hard constraints:** [any additional]
</constraints>

<what_this_is_not>
## What This Is Not
- Not [style or product to avoid]
</what_this_is_not>

<vocabulary>
## Design Vocabulary for This Project
[Auto-populated from research stage]
</vocabulary>
```

Display: `✓ design-context.md written → [path]`

</stage>


<!-- ============================================================
     STAGE 2 — RESEARCH
     ============================================================ -->
<stage id="research">

Display:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► RESEARCHING DESIGN DOMAIN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Skip if `--skip-research` flag is set.

**PRE-LOAD:** Read `design-context.md` in full before starting any research.

## Research Tasks (run in parallel where possible)

### 2a. Reference Analysis

For each reference in `design-context.md`:
- **URL:** Fetch and analyze design decisions
- **Product name:** Web search for design system docs + screenshots
- **Screenshot path:** Read and analyze visually

For each reference, answer:
- Where does the eye land first, second, third?
- What is the spacing philosophy — tight / airy / structured?
- How many colors are active at once? How is accent used?
- What makes the typography feel right?
- What interaction patterns are present?
- What specifically transfers to this project?
- What should be left behind?

### 2b. Domain Pattern Research

Based on the product type from `design-context.md`:
- What are the 3 best-in-class examples of this UI type?
- What layout patterns do they share?
- What components are always present?
- What are the most common mistakes?
- What makes some implementations feel premium?

### 2c. Component Inventory

List every UI component this design will need, with hidden complexity noted.

## Write design-research.md

Write to `$TARGET_FOLDER/design-research.md` with sections:
- Reference Analysis (per reference + cross-reference synthesis)
- Domain Patterns (best-in-class, shared patterns, common mistakes)
- Component Inventory (table: component / purpose / hidden complexity)
- Design Vocabulary (typography, spacing, color, motion decisions)
- Sources table

**After writing:** Update the `<vocabulary>` section in `design-context.md`.

Display: `✓ design-research.md written — [X] references analyzed`

</stage>


<!-- ============================================================
     STAGE 3 — PLAN (Design System)
     ============================================================ -->
<stage id="plan">

Display:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PLANNING DESIGN SYSTEM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PRE-LOAD:** Read both `design-context.md` and `design-research.md` in full before generating.

## Generate Design System (PART 1 — TOKENS)

1. **Color palette** — semantic token names, HSL values, light + dark mode
   - Required tokens: background, surface, surface-raised, border, border-subtle,
     text-primary, text-secondary, text-muted, accent, accent-hover,
     destructive, success, warning, focus-ring

2. **Typography scale** — font family, levels (display/heading/subheading/body/caption/label/code)
   - For each: size in rem, line-height, font-weight, letter-spacing

3. **Spacing scale** — 4px base unit
   - xs=4 / sm=8 / md=16 / lg=24 / xl=32 / 2xl=48 / 3xl=64 / 4xl=96

4. **Border radius scale** — none / sm / md / lg / full

5. **Shadow + elevation** — flat / raised / overlay (CSS box-shadow values)

## Generate Design System (PART 2 — COMPONENTS)

For each component: visual style, all states, Tailwind/CSS classes, rationale.

Required: Primary button, Secondary button, Ghost button, Input, Textarea,
Card (default + interactive), Badge/Pill (semantic variants), Dropdown shell,
Toggle/Switch, plus any domain-specific components from research.

## Generate Wireframes

Using only the components and tokens defined, generate ASCII wireframes for
each screen needed. Annotate spacing and visual hierarchy per screen.

Write to `$TARGET_FOLDER/design-system.md` with sections:
`<tokens>`, `<components>`, `<layouts>`, `<system_gaps>`

Display: `✓ design-system.md written — [X] tokens, [Y] components, [Z] screens`

</stage>


<!-- ============================================================
     STAGE 4 — VERIFY PLAN
     ============================================================ -->
<stage id="verify-plan">

Skip if `--skip-verify` flag is set.

Display:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► VERIFYING DESIGN PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PRE-LOAD:** Read `design-system.md` and `design-context.md` in full.

Check for:
1. Token completeness — any surfaces or states with no token?
2. Token naming — any ambiguous names?
3. Component gaps — anything wireframes reference but system doesn't define?
4. Spacing consistency — scale covers all gaps?
5. Accessibility — any color pair likely to fail WCAG AA 4.5:1?
6. Dark mode readiness — all token names semantic (swappable)?
7. Vocabulary alignment — does system honor research vocabulary?
8. Coherence — does visual language hold across components?

Severity: [BLOCK] = cannot proceed / [WARN] = should fix / [NOTE] = consider it

Fix all [BLOCK] items before proceeding.

Write `$TARGET_FOLDER/design-verify-plan.md`.

Display: `✓ Plan verified — [result] — proceeding to execution`

</stage>


<!-- ============================================================
     STAGE 5 — EXECUTE (3 Passes)
     ============================================================ -->
<stage id="execute">

## Pass 1 — Full Context Dump (~50%)

Display:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PASS 1 — FULL CONTEXT DUMP (~50%)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PRE-LOAD:** Read design-context.md + design-system.md tokens + components + layouts.

Requirements:
- Use ONLY semantic tokens — zero hardcoded hex values
- Use tech stack from design-context.md constraints
- Responsive: mobile-first, desktop at 1280px
- Dark mode via class strategy with semantic tokens (if required)
- Accessible: aria labels, focus rings, keyboard navigation, WCAG AA
- Component extraction: no single file over 200 lines
- Flag any design system gap rather than inventing a solution

Write generated code to `$TARGET_FOLDER/`.
Write `$TARGET_FOLDER/design-pass1-notes.md` with screens built, gaps, decisions.

Display: `✓ Pass 1 complete — moving to self-review`

---

## Pass 2 — Self-Review (~99%)

Display:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PASS 2 — SELF-REVIEW (~99%)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PRE-LOAD:** Read design-system.md + design-pass1-notes.md + current screen code.

Review and fix per screen:
1. Token compliance — every hardcoded value that should be a semantic token
2. Spacing rhythm — scale steps consistent?
3. Typography hierarchy — size jumps correct? Reading order correct?
4. Color balance — accent overused? Neutral range monotonous?
5. Visual alignment — ragged edges or misaligned elements?
6. Accessibility — WCAG AA for all text/background pairs, aria attributes?
7. State completeness — every interactive element has all required states?
8. Design system coherence — looks like it belongs with the other screens?

Write `$TARGET_FOLDER/design-pass2-notes.md` with numbered fix list per screen.

Display: `✓ Pass 2 complete — moving to micro polish`

---

## Pass 3 — Micro Polish (100%)

Display:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► PASS 3 — MICRO POLISH (100%)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PRE-LOAD:** Read design-system.md tokens section + design-pass2-notes.md + specific code.

Surgical changes only — no structural changes. Every change must reference
a value from the token system or pass 2 notes. No creative additions.

Write `$TARGET_FOLDER/design-pass3-notes.md`.

Display: `✓ Pass 3 complete — moving to final verification`

</stage>


<!-- ============================================================
     STAGE 6 — VERIFY (Final)
     ============================================================ -->
<stage id="verify">

Skip if `--skip-verify` flag is set.

Display:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► FINAL DESIGN VERIFICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**PRE-LOAD:** Read design-system.md + design-context.md + design-pass3-notes.md.

Senior designer critique — be harsh. Check:
1. Visual hierarchy — eye goes to right place for core user action?
2. Micro-spacing and alignment — every inconsistency
3. Contrast — WCAG AA failures and near-failures
4. Typographic rhythm — line-heights, letter-spacing, size jumps
5. Cross-screen consistency — what breaks between screens?
6. Token compliance — any hardcoded value?
7. Reference fidelity — matches what was taken from references?
8. Anti-reference check — avoids what it was supposed to NOT feel like?
9. Motion — transitions feel alive or mechanical?

Fix all [BLOCK] items. Re-run critique if needed (max 2 iterations).

Write `$TARGET_FOLDER/design-verification.md` with critique, fixes, sign-off checklist.

</stage>


<!-- ============================================================
     COMPLETION
     ============================================================ -->
<completion>

Display:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD ► DESIGN COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

 Output files:
   [TARGET_FOLDER]/
   ├── design-context.md          ← brief, references, vocabulary
   ├── design-research.md         ← domain patterns, analysis
   ├── design-system.md           ← tokens, components, wireframes
   ├── design-verify-plan.md      ← pre-execution gate
   ├── design-pass1-notes.md      ← pass 1 gaps + decisions
   ├── design-pass2-notes.md      ← self-review fix list
   ├── design-pass3-notes.md      ← micro polish log
   ├── design-verification.md     ← final critique + sign-off
   └── [generated code files]

 Taste sign-off:
   [ ] Eye lands in the right place on every screen
   [ ] Spacing breathes correctly for this user's density needs
   [ ] Accent is restrained — intentional not decorative
   [ ] Token system is clean — no hardcoded values in code
   [ ] Looks like one designed system not AI output

 To resume later:
   /gsd:design-ui --folder [path] --pass2   (re-run from self-review)
   /gsd:design-ui --folder [path] --pass3   (re-run from micro polish)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

</completion>
