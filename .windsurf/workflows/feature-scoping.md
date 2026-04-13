---
description: Scope a feature before implementing it
---

Before implementing anything non-trivial, complete these steps and
present the result to the user for approval.

1. State the goal.
   One sentence: what the user asked for.

2. Read the behavioral contracts and project memory.
   Read:
   - `AGENTS.md`
   - `UI-STANDARDS.md` (if the task touches UI)
   - `ai_project_manager_kickstart/project/brief.md`
   - `ai_project_manager_kickstart/project/architecture.md`
   - `ai_project_manager_kickstart/project/conventions.md` (if it exists)
   - `ai_project_manager_kickstart/project/file-map.md`

3. Identify the change pattern.
   Examples:
   - model or state property change
   - global setting or config change
   - rendering or output change
   - UI control or workflow change
   - import or export change
   - performance or hot-path change
   - new module or feature
   - bug fix

4. List affected files.
   Search the source tree to confirm the likely touch points.

5. Identify ownership boundaries.
   Name which layer or module should own:
   - user interaction
   - orchestration or coordination
   - state mutation
   - rendering or output
   - persistence
   - import or export

6. Check for existing patterns.
   Do not invent new constants, configs, or abstractions if one
   already exists in the codebase.

7. Check efficiency risk.
   Does the change touch any hot path?
   - render loop
   - recalculation loop
   - serialization
   - import or export
   - background processing

8. Estimate scope.
   How many files changed? Is this still a small design slice?

9. Present scope to the user.
   Show:
   - goal
   - change pattern
   - affected files
   - ownership boundaries
   - efficiency risks
   - estimated size
   - open questions

10. Wait for approval.
    Do not write code until the user confirms.
