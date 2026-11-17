## What it does

`complete-plan` turns software requirements into a complete implementation plan that you can read by responsibility. The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) handles the research, ordinary technical choices, work packages, and acceptance coverage in one invocation. Complex plans have an overview, design topics, and work packages; a small task can stay in one file.

It covers the entire scope you requested, including final integration. Writing a plan does not authorize product changes, dependency installation, issue creation, or a migration.

## When to reach for it

Type `/complete-plan` to invoke it. This is a user-invoked skill: the agent does not start it automatically, and other skills may recommend it but cannot invoke it.

| What you need | Use |
| --- | --- |
| A software implementation plan, a technical proposal, or requirements organised into executable work packages | `complete-plan` |
| A long plan reorganised so you can find the design and acceptance details for one work package | `complete-plan` |
| An existing plan repaired so it covers the original requirements | `complete-plan` |
| An in-depth interview to stress-test your thinking | [grilling](https://aihero.dev/skills-grilling) |
| Throwaway code to answer one design question | [prototype](https://aihero.dev/skills-prototype) |

## Prerequisites

No issue tracker or setup skill is required. Existing code, tests, and project documents make the plan more specific; when they cannot be inspected, the plan marks assumptions and the checks needed before implementation.

A saved plan follows your requested location or the project's convention. Without either, a directory plan starts at `docs/plans/<topic>/README.md`; a small single-file plan uses `docs/plans/<topic>.md`. Explicit requests for one file or a chat-only response take precedence. Simulated content is never described as already saved.

## Reading the plan

The overview holds the requirements, their source, the full route through the work, dependencies, and unresolved delivery decisions. It links to details rather than repeating them. Design paths below are relative to the plan directory, so `design/` sits next to the overview and `tasks/`.

| File | What belongs there |
| --- | --- |
| `README.md` | Goal, requirement identifiers, completion criteria, full roadmap, dependencies, navigation, and links to blockers |
| `design/README.md` | Design index, questions answered by each file, reading order, existing project references, and reasons optional topics are absent |
| `design/glossary.md` | New or changed domain terms, definitions, scope, relationships, evidence, status, and the work that will merge them into the project glossary |
| `design/architecture.md` | Current system, module responsibilities, dependency directions, main data flow, reuse, and necessary structural changes |
| `design/flows.md` | Business flows, state transitions, normal and failure paths, and relevant cancellation, retry, timeout, or concurrency behavior |
| `design/contracts.md` | Interfaces, events, inputs, outputs, validation, errors, permissions, idempotency, and compatibility |
| `design/data.md` | Entities, persisted fields, relationships, constraints, storage, lifecycle, consistency, and necessary migration or recovery |
| `design/ui.md` | Pages, navigation, interactions, loading, empty and error states, feedback, and relevant accessibility or adaptation |
| `design/decisions.md` | Important choices, rationale, alternatives, consequences, assumptions, validation actions, and unresolved decisions |
| `tasks/<work-package>.md` | Deliverable behavior, requirement identifiers, dependencies, required design reading and its purpose, implementation scope, and local acceptance |
| `acceptance.md` | Requirement and work-package coverage, end-to-end integration, regressions, and necessary migration or rollout validation |

Generate the design topics that the work needs. There is no catch-all `design.md`, and no empty file just to satisfy a template. The design index explains which optional topics do not apply. A design topic may split further by module when that helps separate readers find their work.

Each detailed definition has one owner. Other files link to it; a work package links directly to the design needed to implement it. Work-package checks do not replace final integration acceptance.

## Test planning

The overview records the documented development stage, agreed seams, and retained test scope under the shared [TDD policy](https://aihero.dev/skills-tdd#test-scope-and-budget). Unspecified code is unstable and retains only representative happy-path tests. The whole project shares a 15% ideal upper bound and a 30% hard limit for test code, including test-only support. Work packages inherit this budget rather than each receiving an allowance.

Acceptance still covers all requirements, including failures and permissions. Where a retained automated case is outside the current stage, the plan names direct checks or temporary reproductions instead. Reduced retained test scope never removes required implementation or verification.

## Complete delivery, one entry point

A phase schedules work; it does not remove requirements. Every requirement needs an implementation path and an acceptance check. An explicitly limited prototype keeps that limited scope rather than acquiring unrelated production infrastructure.

Ordinary technical choices belong to the agent. It asks only when an unresolved choice materially changes delivery and there is no safe default consistent with the requirements. An unanswered critical question stays visible as a condition. Reorganising a plan preserves requirement and work-package identifiers, decision evidence, and known links; it does not create a second conflicting authoritative plan.

## Common questions

**Why split the design instead of putting everything in one file?**

A reader implementing one work package should be able to find its constraints without reading every other design. The overview supplies the route; the design index names each file's purpose. Multiple modules, phases, work packages, or shared design topics are reasons to use the directory structure. File count and line count do not decide it.

**Does every plan need every design file?**

No. A small, focused task can be a single file. In a directory plan, generate only the relevant design topics and explain omissions in the index. A backend-only change need not have a UI file. Your explicit request for one file is respected, with headings and navigation instead of sibling files.

**Is the new glossary another copy of the project glossary?**

No. It records only additions or semantic changes. Stable terms link to the current project definitions. Changed terms cite the old definition and explain the change; uncertain definitions stay labelled. The plan identifies where and through which work package the additions will be merged. Planning alone does not authorize editing the project glossary or global ADRs.

**Does requesting the plan start implementation or create issues?**

No. You receive the plan and its reading entry point, or its contents in chat when requested. Building the feature, changing product code, or publishing issues needs separate authorization. You do not need tracker setup or a hand-off through the interview and spec skills.

## It's working if

- Every original requirement maps to a work package and an acceptance check, ending at complete integration.
- The overview leads you to the work package you need, and that work package links to its required design.
- Design files have distinct responsibilities, usable links, and no copied definitions that can drift apart.
- New or changed terminology is separate from stable project definitions, with evidence and uncertainty visible.
- Irrelevant design topics are explained rather than represented by empty files.
- Ordinary technical choices stay with the agent, and unresolved irreversible choices stay visibly conditional.
- You receive the plan without product changes, issue creation, or another required skill invocation.

## Where it fits

This is a standalone engineering planning entry point. [grill-with-docs](https://aihero.dev/skills-grill-with-docs) is the neighbour for an interview that also maintains domain documents; [to-spec](https://aihero.dev/skills-to-spec) packages an already-developed conversation for a tracker-based flow. Neither is required before this skill. [ask-matt](https://aihero.dev/skills-ask-matt) maps the alternatives.
