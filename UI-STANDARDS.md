# UI Standards

This file contains the full UI, usability, and accessibility rules for
the project. `AGENTS.md` references this file. Read it before any task
that touches UI, controls, layout, text, states, accessibility, or
user-facing behaviour.

---

## Design system

IBM Carbon Design System is the **reference standard** for this
project. Carbon is not installed as a package dependency. All UI
components are implemented in the project's own code to match Carbon's
productive design language: component anatomy, interaction behaviour,
spacing, sizing, and visual conventions.

### Carbon-first UI discipline

- Prefer Carbon components, patterns, tokens, spacing, and interaction
  conventions wherever a suitable Carbon solution exists. Do not invent
  a custom control if Carbon already provides an appropriate one.
- Use Carbon's **productive** UI style for the working interface, not
  expressive or marketing styling.
- Use semantic design tokens for colour, spacing, typography, layer,
  border, and state. Do not hard-code ad hoc UI values unless there is
  no suitable tokenised equivalent.
- Keep layouts modular, consistent, and task-focused. Reuse an existing
  Carbon pattern before creating a new one.
- Where Carbon defaults meet AA but not this project's stricter AAA
  target, adapt them. Carbon is the baseline, not the ceiling.

### Token systems

Three token systems run side by side in `styles/tokens.css`:

| System | Prefix | Governs | Examples |
| --- | --- | --- | --- |
| **UoN brand palette** | `--uon-` | Source brand colours, UI surfaces, interactive states, text hierarchy, support/feedback colours, links | `--uon-blue`, `--uon-nottingham-blue`, `--uon-jubilee-red`, `--ui-01`…`--ui-05`, `--text-01`…`--text-05`, `--support-error`, `--interactive-01` |
| **Okabe-Ito map palette** | `--map-series-` | Canvas/data series colours (colour-blind safe), map rendering tokens (ink, marker, path, label, selection) | `--map-series-0`…`--map-series-8`, `--map-ink`, `--map-label-text` |
| **Carbon-aligned structural** | `--space-`, `--text-`, `--control-`, `--radius-`, `--motion-`, `--elev-` | Spacing scale, typography scale, control sizing, border widths, radii, elevation, motion timing, focus ring | `--space-1`…`--space-8`, `--text-xs`…`--text-xl`, `--control-h`, `--radius-2`, `--motion-fast`, `--elev-2` |

**Rules:**

- Do **not** collapse these systems into one. Each serves a distinct
  purpose (brand identity, data accessibility, structural layout).
- UoN tokens define **what colour** — Carbon-aligned tokens define
  **how much space, how big, how fast**.
- Okabe-Ito tokens are used **only on the canvas** for data series.
  Never use `--map-series-*` for UI chrome.
- When adding a new token, decide which system owns it based on the
  table above. If it's a brand colour → `--uon-`. If it's a canvas
  data colour → `--map-series-`. If it's spacing, sizing, or
  structural → use the Carbon-aligned prefix.
- The file also contains legacy compatibility aliases (e.g.
  `--primary`, `--color-primary`). Do not add new aliases. Migrate
  away from them when touching related code.
- `@media (prefers-contrast: more)` and `@media (prefers-reduced-motion: reduce)` overrides are defined at the bottom of `tokens.css`. Respect these when adding new tokens.

---

## Usability heuristics

The following rules are derived from Nielsen's usability heuristics.
They are **hard operating rules**, not aspirational guidelines.

### Match between the system and the real world

- Prefer user language over internal or technical jargon.
- Use words, phrases, and concepts familiar to the user.
- Follow real-world conventions and natural ordering.
- Labels, messages, and control names must describe the task the user is
  actually trying to do, not the implementation beneath it.

### Carbon content and form rules

- Use **sentence case** for UI text.
- Every input must have a visible label.
- Visible label text must be reflected in the accessible name.
- Labels must be concise and clear; prefer 1–3 words where practical.
- Do not use colons after labels.
- Use helper text only when it prevents error, clarifies format, or
  explains consequence.
- Prefer native HTML form controls before custom ARIA-heavy controls.
- Group related controls with clear headings and structure.

### Visibility of system status

- Every async or delayed action must show status immediately:
  loading, progress, success, or error.
- The UI must never appear frozen during processing.
- Long-running work must show an explicit loading state, and heavy
  operations should show progress where possible.
- Important status changes must be announced programmatically where
  relevant, not only shown visually.
- Auto-save, export, import, render, and recovery states must be visible
  and understandable.

### Empty, loading, and no-data states

- Every major panel, canvas, or workspace must have an intentional empty
  state.
- Empty states must explain what belongs here and what the user can do
  next.
- Loading states must indicate that work is in progress and preserve
  layout stability where practical.
- No-data states must distinguish between "nothing here yet", "filtered
  out", "failed to load", and "not available".
- Do not leave blank panels, unexplained placeholders, or silent failure
  states.

### User control and freedom

- Provide cancel, close, back out, or undo routes for any non-trivial
  action.
- Do not trap the user in transient modes, overlays, or incomplete
  flows.
- Keyboard escape routes must remain available where appropriate.
- Destructive actions must have clear confirmation or a reliable undo
  path.
- The user must be able to recover from accidental actions without
  having to reload or lose work.

### Consistency and standards

- Use the same words, icons, control patterns, spacing logic, and
  interaction rules for the same concepts throughout the app.
- Follow existing project event names, UI terms, Carbon conventions, and
  established design tokens.
- Do not create synonyms for existing concepts.
- Similar panels and controls should behave the same way unless there is
  a clear documented reason for divergence.

### Error prevention

- Prevent errors before they happen: constrain invalid input, validate
  early, and disable impossible actions.
- Prefer safe defaults over blank or dangerous defaults.
- For critical submissions, destructive actions, or irreversible changes,
  provide validation, confirmation, reversal, or a combination of these.
- Do not allow invalid states to propagate silently through the UI.

### Recognition rather than recall

- Keep key controls visible when needed.
- Do not rely on users remembering hidden modes, keyboard shortcuts,
  required formats, invisible constraints, or prior state.
- Show current selection, active mode, current tool, and important state
  changes explicitly in the interface.
- Surface useful context near the point of action rather than forcing
  the user to remember information from elsewhere.

### Flexibility and efficiency of use

- Support both novice use and efficient repeat use.
- Preserve sensible defaults, but expose efficient shortcuts for common
  actions.
- Reduce unnecessary clicks, repeated input, and mode switching.
- Where the platform supports multiple input types, do not force users
  into a single input modality.
- Avoid drag-only interactions; provide click, tap, and keyboard
  alternatives where practical.

### Aesthetic and minimalist design

- Keep interfaces lean and task-relevant.
- Do not add decorative chrome, redundant copy, visual noise, or
  competing calls to action.
- Motion, colour, and visual emphasis must support task completion, not
  distract from it.
- Dense tools are acceptable where needed, but clutter is not.

### Help users recognize, diagnose, and recover from errors

- Error messages must say what happened, where relevant, and what the
  user should do next.
- Errors must be specific, human-readable, and linked to the relevant
  field or control both visually and programmatically.
- Do not use vague failures such as "Something went wrong" without
  actionable detail where detail can safely be provided.
- Where recovery is possible, the UI must point directly to the recovery
  route.

### Help and documentation

- Provide contextual help for non-obvious controls, workflows, or modes.
- Help content must be task-focused, concrete, and brief.
- Prefer inline guidance, helper text, and tooltips over forcing users
  into external documentation for routine actions.
- Use headings and structure so help content can be scanned quickly.

### Motion and attention discipline

- Motion must be subtle, purposeful, and easy to ignore.
- Non-essential motion should be reduced or disabled when possible.
- Respect `prefers-reduced-motion`.
- Do not use motion as the only carrier of meaning.
- No content may flash more than three times in any one-second period.

---

## Accessibility — WCAG 2.2 AAA by default

This project targets **WCAG 2.2 AAA by default for all applicable UI
and content**. Do not claim blanket AAA conformance unless all
applicable success criteria are actually met. Use **AAA by default,
exceptions documented** as the working rule. Where a criterion cannot
reasonably apply or must be excepted, record the exception explicitly
in change notes or implementation notes.

### Perceivable

- Text and images of text must meet **7:1 contrast**.
- Large-scale text may use **4.5:1** only where WCAG permits it.
- Do not rely on colour alone to convey state, status, meaning, or
  required action.
- Avoid images of text unless essential.
- Link text must make sense on its own; avoid vague text such as "more",
  "here", or "click here".
- Use headings and landmarks to structure substantial content and
  control-heavy panels.
- Provide text alternatives for meaningful non-text content.

### Operable

- All functionality must be keyboard operable without traps.
- Focus order must be logical and must preserve meaning.
- Focus indicators must be clearly visible, high-contrast, and not
  obscured by sticky headers, overlays, or custom panels.
- Pointer targets should be at least **44 × 44 CSS px** unless a WCAG
  exception clearly applies.
- Do not require path-based gestures, dragging, hovering, or fine motor
  precision when a simpler alternative can be provided.
- Interaction-triggered motion must be avoidable when non-essential.
- Provide pause, stop, or hide controls for moving, blinking, scrolling,
  or auto-updating content when applicable.
- Warn clearly before any timeout that could cause data loss, and
  preserve work where practical.

### Understandable

- Use predictable behaviour and consistent placement.
- Do not change context unexpectedly on focus, input, or selection
  unless clearly signposted and user-initiated.
- Form instructions, validation, and recovery guidance must be clear and
  placed near the relevant control.
- Visible labels and accessible names must match closely enough to
  support speech input and assistive technology use.
- Use user-facing terminology, not internal implementation terms.

### Robust

- Prefer semantic HTML before ARIA; no ARIA is better than bad ARIA.
- Use ARIA only when native semantics do not provide the required
  meaning or behaviour.
- Dynamic updates such as loading, validation, save status, and errors
  must be exposed programmatically where relevant.
- Custom widgets must expose role, name, value, and state correctly.

---

## Design review gate

Before sign-off on any UI-affecting change, verify:

1. Which Carbon component or pattern this change follows.
2. Why a custom pattern was necessary if Carbon was not used.
3. Which Nielsen heuristics were most at risk.
4. Text contrast meets **7:1** for normal text and **4.5:1** for large
   text where permitted.
5. Focus order, focus visibility, and focus non-obscuration still work.
6. All pointer targets meet **44 × 44 CSS px** unless a documented WCAG
   exception applies.
7. Visible labels match accessible names.
8. Link text is self-describing without surrounding context.
9. Empty, loading, success, validation, and error states were all
   considered and are not visual-only.
10. Keyboard, pointer, and assistive-technology routes all still work.
11. Motion can be reduced or disabled where non-essential.
12. Critical submissions or destructive actions support validation,
    confirmation, undo, or reversal as appropriate.
13. Any exception to the AAA-by-default rule is documented explicitly.
