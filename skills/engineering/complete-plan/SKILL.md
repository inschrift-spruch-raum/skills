---
name: complete-plan
description: 'Complete implementation plans with an overview, design topics, work packages, and acceptance coverage. Small tasks may use one file; planning only.'
disable-model-invocation: true
---

# Complete Implementation Plan

The user describes the goal; the agent turns it into a complete plan that can be implemented and verified. Independently organize requirements, do the necessary research, choose an approach, break down the work, and check coverage before delivering the documents.

## Delivery contract

- **Complete delivery** follows the user's original requirements and explicitly agreed changes. Include the integration, error handling, compatibility, and verification needed to make those requirements usable. Choose supporting work to fit the project, without adding unrelated features or enterprise architecture.
- **Phases determine sequence.** Size, effort estimates, and context limits change how work is divided, not its scope. Plan every phase through complete delivery; the first runnable slice is not the endpoint. Excluding, deferring, or simplifying an existing requirement needs explicit user authorization and a recorded reason.
- When the user explicitly limits the work to a prototype, proof of concept, or particular version, cover that scope fully. Internal experiments can resolve uncertainty, but the subsequent implementation, integration, and acceptance remain part of the complete delivery plan.
- **One entry point, layered delivery.** This skill is self-contained. Ordinary planning requires no prior interview, spec conversion, ticketing skill, issue tracker setup, session switch, or approval at every step. Organize complex plans into an overview, design topics, and work packages; small tasks may use one file. Do not require issues, a project-wide glossary, ADRs, or duplicate documents.
- **Planning does not authorize implementation.** When the user asks only for a plan, write plan documents without changing product code, installing dependencies, publishing issues, or executing migrations. If the user also requests implementation, proceed within that authorization.

## Steps

### 1. Organize requirements and do the necessary research

Read the current conversation, supplied materials, and relevant existing plans first. In an existing project, inspect the entry points, conventions, code, tests, and build configuration directly related to the requirements. Reuse existing findings instead of repeating an interview. Consult external primary sources when needed. Investigate unknowns that affect the approach, rather than exploring the whole repository or researching to fill a template.

Give each user requirement a short identifier, preserve its source or original meaning, and distinguish verified facts, technical choices, and assumptions to validate. When code or materials are inaccessible, continue from available information and mark what needs verification and how to check it before implementation. Do not invent inspected files, verified commands, or system behavior.

Completion criterion: all explicit requirements and hard constraints are recorded, with enough evidence to choose the main implementation path. Material ambiguities proceed to the next step.

### 2. Own technical decisions and escalate only material tradeoffs

| Situation | Action |
| --- | --- |
| A fact can be found in code, tests, documentation, or tools | Verify it independently instead of asking the user. |
| A local, reversible implementation choice has an established project convention | Choose the best fit and record only reasons that matter to later work. |
| A function or branch detail does not determine an external contract, risk, or feasibility | Leave it for implementation and testing; do not make it a planning blocker. |
| An unresolved tradeoff would materially change delivery | Apply the question threshold below. |

Ask the user only when all of the following are true:

1. The current conversation, accessible materials, and reasonable research cannot settle the question.
2. The answer changes scope, user-visible behavior, major cost, data safety, or an irreversible compatibility tradeoff.
3. There is no reasonable default that is both safe enough and consistent with known requirements; choosing would make an unauthorized major decision for the user.

Batch independent material questions, provide a recommendation and its effects, and first complete unaffected research and planning. Do not interrogate the user about each function boundary, name, or internal type. Define business boundaries that determine user-visible outcomes, but ask about their meaning only when evidence is missing and the threshold above is met.

If the user does not answer, retain the unresolved condition and affected tasks. Deliver a conditional plan clearly marked as awaiting a critical decision. Silence or a skipped question is not consent; removing requirements does not make a plan complete or executable. Do not ask again about choices the user has already made explicit.

Completion criterion: the agent owns ordinary technical choices. Any remaining disagreement genuinely requires user judgment, or there is reliable evidence to continue planning.

### 3. Write a plan ready for implementation

Prefer the user's requested format. Use a directory plan for multiple modules, phases, work packages, or shared design topics; a small, focused task with few work packages may use one file. For a directory plan, first read [PLAN-STRUCTURE.md](PLAN-STRUCTURE.md) and follow its file responsibilities: `README.md` is the roadmap entry point, shared design topics live under the plan's `./design/`, tasks under `tasks/`, and overall acceptance in `acceptance.md`. Keep design topics separate instead of collecting them in `design.md`. Fixed lengths and empty templates do not establish completeness.

- **Goal and scope**: what the user will be able to do, requirement identifiers, hard constraints, and overall completion criteria. Exclude only work outside the requirements or explicitly excluded by the user.
- **Current state and approach**: verified facts, the chosen implementation path, key module or interface contracts, reuse, and necessary changes. Include useful implementation locations. Go into function-level constraints only when they affect behavior, safety, or feasibility.
- **Work packages**: deliverable behavior, covered requirement identifiers, dependencies, required design links and their purpose, implementation boundaries, and acceptance. Size each package so it can be assigned and verified; implementation agents own ordinary internal details. Work across sessions or implementers still belongs to one complete roadmap.
- **Overall acceptance**: requirement-level checks, end-to-end integration, relevant regressions, and necessary migration or rollout validation. Supply evidence-based check entry points or test scenarios. Do not present unverified commands as runnable. Passing every work package does not by itself establish overall acceptance.
- **Risks and assumptions**: retain only risks, validation actions, and unresolved decisions that affect implementation, linked to the relevant work. Avoid unrelated assumptions.

Keep the requirements' original meaning in the overview. Tasks reference requirement identifiers, and overall acceptance links requirements to tasks. Each detailed design has one owning topic file; other files link to it. In directory plans, put new or semantically changed terms in `design/glossary.md`, recording definitions, scope, relationships, evidence, status, and how implementation will merge them into the project glossary. Stable terms reference existing definitions. Explain absent optional topics, such as a glossary with no additions, in the design index instead of creating empty files. Single-file and chat-only plans retain sections with the same responsibilities. Independent work can run in parallel; sequential phases must lead to final integration and complete delivery.

Completion criterion: an implementer can start from the plan without repeating requirements discovery. Every requirement has an owner, acceptance evidence, and a path to overall delivery.

### 4. Check and revise before delivery

The agent performs these checks without adding user approval gates:

- Compare against the **original requirements and authorized changes**, mapping each to work and acceptance. Checking only an agent-written reduction of the scope is insufficient.
- Find unfinished requirements hidden behind a "first version," "later," placeholders, or demonstrations, and restore their implementation path. These words are not prohibited; actual scope determines completeness.
- Check that someone owns cross-module integration and necessary supporting work, and that completing all tasks will satisfy the overall completion criteria.
- Take back any verifiable facts, ordinary technical choices, or function details pushed onto the user. Remove requirements to invoke another skill just to receive the plan.
- Distinguish verified facts, assumptions, and critical unresolved decisions. State blockers and their effects without claiming unconditional executability.
- Follow links from the overview through a work package to its required design and acceptance. Check the design index, relative links, requirement and work-package identifiers, and dependencies. Repair omissions, duplicate definitions, and empty files. Make additions and changes to authoritative terminology explicit through references.

Completeness checking is part of planning, not a mandatory separate review process.

### 5. Save and deliver

Use the user's requested location, otherwise follow the project's document conventions. Without either, save a directory plan at `docs/plans/<topic>/README.md` with its design, task, and acceptance files alongside it; save a small single-file plan at `docs/plans/<topic>.md`. Choose a short name without asking the user about filenames or locations. If the user asks for chat-only delivery, respond in chat without writing files or claiming they were saved. When revising a plan, preserve context, requirement identifiers, and unresolved decisions. Structural changes must update the old entry point, valid links, tasks, and acceptance without leaving conflicting versions.

Finish with the overview link, suggested reading order, a brief description of the approach and full delivery scope, and any actual critical unresolved decisions. Deliver the completed plan without asking the user to approve already settled steps or manually chain other skills.

## Maintenance validation

When changing this skill, use the scenarios in [EVALS.json](EVALS.json) to validate scope coverage, file responsibilities, navigation, question thresholds, and the single-entry-point behavior. Ordinary planning does not require reading that file.
