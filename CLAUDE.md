# CLAUDE.md

> This file is the behavioral guide for Claude Code working in this repository. It takes priority over default behaviors — in case of conflict, this file wins.

---

## Language & Communication

- Conversations default to **Chinese (中文)**
- Code comments must be in **Chinese** — all functions and key logic blocks require comments

---

## Documentation Policy (Hard Constraint)

**Do NOT create any new documentation files by default**, including but not limited to:

- `README.md`, design docs, usage guides, architecture docs
- Any new `.md` files or annotative documentation

**Only create or modify documentation when the user explicitly asks** for it (e.g., "write docs / generate README / create documentation").

---

## Core Principles

- **Simplicity First** — Keep every change as simple as possible. Minimize scope of impact.
- **No Laziness** — Find root causes, not patches. Execute at a senior developer standard.
- **Minimal Impact** — Only change what's necessary. Avoid introducing new issues.
- **Separation of Intent** — Handle one intent at a time: explore, decide, execute, or review — never mix them.
- **DB Change Traceability** — Any schema change must be accompanied by a SQL migration record for iterative upgrades.

---

## Workflow Orchestration

### 1. Progressive Spec: Complexity-Based Approach

Different complexity levels follow different depths of process — incidental complexity must be minimized:

| Scope | Process |
|-------|---------|
| Simple (field change, bug fix) | Execute directly, no Spec needed |
| Medium (3+ steps, architectural decisions) | Lightweight Spec → HARD-GATE → Code |
| Complex (cross-module, multi-system) | Full Propose → Apply → Review |

**Spec Three Iron Rules** (triggered for medium+ complexity):

1. **No Spec, No Code** — No writing code without an approved Spec.
2. **Spec is Truth** — When Spec and code conflict, the code is wrong.
3. **Reverse Sync** — On deviation during execution, fix the Spec first, then fix the code.

**HARD-GATE**: After generating a complete Spec, wait for the user's **explicit confirmation** before writing any code. No code modifications before confirmation.

**Research Must Cite Sources**: Every conclusion about the current codebase must reference `file path + function name`. No "typically" or unsupported assertions.

**Spec Section-by-Section Confirmation**: Don't generate the entire Spec at once. Output section by section (current state analysis → features → risks & decisions), waiting for user confirmation after each. The earlier a direction mismatch is caught, the cheaper it is to correct.

### 2. Plan Mode Default

- Any medium+ complexity task defaults to **plan mode**.
- Stop and re-plan immediately on issues — never force through.
- Plan mode applies to the **verification phase** too, not just the build phase.

### 3. Subagent Strategy

- Use subagents heavily to keep the main context window clean.
- Delegate research, exploration, and parallel analysis to subagents.
- Invest more compute into complex problems via subagents.
- **Each subagent does exactly one thing** — focused execution.

### 4. Execution Freedom Curve

| Phase | Freedom | Principle |
|-------|---------|-----------|
| Research | Medium | Explore freely, but conclusions must cite code sources |
| Design | High | Imagine fully, offer options + recommendations |
| Planning | Low | Precise down to file paths and function signatures |
| Execution | **Zero** | Follow the plan strictly; stop and ask on any deviation |
| Verification | Medium | Check freely, but conclusions need evidence |

### 5. Verification Iron Rules

**No task may be marked complete without verification.**

- Must present verifiable evidence: build output / test results / runtime logs.
- "Should be fine" or "theoretically works" are not acceptable without proof.
- Compare behavior before and after changes when necessary.
- **Backend**: Must pass unit tests to be considered complete.
- **Frontend**: Must write test cases and pass Playwright e2e tests to be considered complete.

### 6. Demand Elegance (In Moderation)

- For non-trivial changes, pause and ask: "Is there a more elegant approach?"
- If a solution feels hacky: understand the constraints, then implement the elegant version.
- **Simple and obvious fixes — just do them. Don't over-engineer.**

### 7. Autonomous Bug Fixing

- Given a bug report, go fix it — don't wait for hand-holding.
- Pointed to logs, errors, or failing tests? Solve them.
- No need for the user to switch context.
- CI test failures — fix them proactively.

---

## Task Management

1. **Write the plan first** — Log tasks in chronological order into `tasks/todo.md` using checkable task items, e.g.:
   ```
   ## yyyy-MM-dd describe the plan
   
   - [ ] Step 1: describe the task
   - [ ] Step 2: describe the task
   - [x] Step 3: completed task
   ```
2. **Confirm before executing** — Medium+ complexity: start implementation only after HARD-GATE.
3. **Track progress** — Mark items done immediately upon completion.
4. **Explain changes** — Provide a high-level explanation for each step.
5. **Record results** — Add a review section at the end of `tasks/todo.md`.
6. **Capture lessons** — Update `tasks/lessons.md` after user corrections.

---

## Self-Improvement Loop

- After each user correction, record the pattern in `tasks/lessons.md`.
- Add corresponding rules to prevent similar errors from recurring.
- **Review relevant rules from lessons at the start of every session.**
- Proactively suggest capturing valuable pitfalls and domain discoveries into the project knowledge base.
