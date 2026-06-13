---
name: serious-mode
description: Intent clarification, codebase assessment, detailed planning, and checklist-driven implementation for non-trivial coding tasks. Use when asked to build, change, refactor, fix, or implement something and the request benefits from clarifying vague intent, inspecting the relevant current codebase, pausing for crucial development decisions, drafting acceptance/testing/success criteria, and using goal tracking before implementation. Do not use for trivial one-command answers or when the user explicitly asks only for a quick explanation.
---

# Serious Mode

## Overview

Use this skill to turn vague or substantial coding requests into clear, inspected, planned, and verified implementation work. Keep the implementation logic as simple as the clarified requirements allow.

Right-size the workflow. For small local changes, produce compact artifacts. For risky or cross-cutting changes, be explicit and pause at decision gates.

## Workflow

### 1. Break Down Intent

Before editing files, decompose the user's prompt into:

- **Requested outcome**: what the user wants changed or produced.
- **Actual intent**: the underlying problem or workflow the request appears to solve.
- **Scope**: what is in scope, out of scope, and unknown.
- **Constraints**: explicit user requirements, repo conventions, architecture boundaries, time/budget limits, and tool limitations.
- **Ambiguities**: wording that could lead to meaningfully different implementations.

Clarify vague language into concrete behavior. If the user asks only for a plan, review, explanation, or investigation, do not implement unless they later ask for implementation.

### 2. Gather Relevant Codebase Context

Inspect the current codebase before planning. Prefer targeted context over broad reading:

- Check repository state and existing user changes when relevant.
- Search for related files, tests, models, contracts, dependencies, routes, and feature folders.
- Read the current implementation and nearby tests before deciding changes.
- Identify existing patterns and architecture boundaries to preserve.
- Note any current behavior that conflicts with the prompt.

Do not edit files during this step except for harmless generated scratch artifacts explicitly needed for investigation.

### 3. Produce An Agent Work Report

Prepare a concise internal work report for guiding implementation. Do not expose hidden chain-of-thought; summarize conclusions when communicating with the user.

Include:

- Original prompt and clarified intent.
- Assumptions being made.
- Relevant current codebase findings.
- Files likely to change and why.
- Risks, unknowns, and blocked points.
- Simplicity rule for the implementation.
- Testing surfaces and commands likely to prove the change.

### 4. Pause For Crucial Development Decisions

Pause and ask the user before continuing when a decision could materially affect product behavior, data, contracts, architecture, dependencies, security, cost, or future maintenance.

Ask only for crucial decisions. Do not ask for routine implementation choices that can be inferred from the codebase.

Use this format:

- State the decision needed in one sentence.
- Provide 2-3 mutually exclusive options.
- Mark one option as **Recommended**.
- Explain the tradeoff of each option briefly.
- After the user answers, record that choice as a requirement and use it to guide implementation.

Pause especially for:

- Public API, schema, persistence, migration, or data-loss changes.
- Authentication, authorization, privacy, payment, or security behavior.
- New runtime dependencies, toolchain changes, or version upgrades.
- Cross-layer architecture changes or boundary violations.
- User-visible behavior where multiple interpretations are plausible.
- Performance/cost tradeoffs or background job behavior.
- Removing tests, weakening validation, or changing accepted failure behavior.

### 5. Formulate The Plan

Create a step-by-step plan after the intent and codebase context are clear. The plan must be specific enough that another agent could follow it.

Include:

- **Clarified Goal**: concrete target behavior.
- **Current Situation**: relevant codebase findings.
- **Files To Change**: existing files and likely new files, with purpose.
- **Implementation Steps**: ordered steps with simple logic unless complexity is explicitly required.
- **Acceptance Checklist**: observable behaviors the result must satisfy.
- **Review/Testing Checklist**: commands, manual checks, review points, and edge cases.
- **Success Criteria**: what must be true to consider the work complete.
- **Risk Controls**: assumptions, rollback points, and unresolved risks.

### 6. Apply The 90 Percent Readiness Gate

Proceed to implementation only when the plan is likely to work with high confidence.

Treat the plan as ready when:

- The user intent is concrete enough to implement.
- Relevant existing code and tests were inspected.
- The files to change are identified.
- Critical user decisions are answered or not needed.
- The implementation path is simple and compatible with repo patterns.
- Acceptance and testing criteria are clear.
- No known blocker remains.

If readiness is below this threshold, gather more context or pause for the user instead of guessing.

### 7. Start Goal-Backed Implementation

When the plan passes the readiness gate and implementation is requested, start goal tracking if the runtime supports it.

- Use `/goal` or the available goal-management tool with a concrete implementation objective.
- Keep the acceptance checklist, review/testing checklist, and success criteria as the execution guide.
- If goal tooling is unavailable, continue using the plan explicitly and note that no goal tool was available.

Do not start goal tracking for plan-only, review-only, or explanation-only requests.

### 8. Implement Against The Plan

Implement step by step:

- Edit only the files required by the plan unless new findings force a plan update.
- Preserve unrelated user changes.
- Keep logic direct and readable.
- Prefer existing repo patterns over introducing abstractions.
- Update or add tests where the acceptance checklist requires proof.
- If a new crucial decision appears, stop and ask before continuing.

### 9. Verify And Close

Use the checklists before final response:

- Run the planned tests or explain exactly why they could not be run.
- Confirm each acceptance item is satisfied.
- Perform the review/testing checklist.
- Compare the result against the success criteria.
- Report files changed, tests run, remaining risks, and any follow-up needed.

If verification fails, fix the issue or clearly report the blocker rather than claiming completion.
