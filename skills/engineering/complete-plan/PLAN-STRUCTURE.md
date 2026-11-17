# Plan Directory and File Responsibilities

Use this structure for directory plans. All paths are relative to the root of one plan. For example, the default root is `docs/plans/<topic>/`, and `./design/` holds that plan's design material.

## Choose the delivery format

- Honor the user's requested file, format, location, or chat-only delivery first. Chat responses retain the same content responsibilities without claiming that files were saved.
- Use a directory plan for multiple modules, phases, independently assignable work packages, or shared design topics. The overview provides one reading entry point; details expand by responsibility.
- A small, focused task with few work packages and no separate shared design topics may use one file. Retain goal, approach, tasks, acceptance, and necessary risks without creating empty files to fill a directory.
- Split according to meaning and readers' needs, not a fixed line count, word count, or file count. When the user explicitly requests one file, use headings, a table of contents, and anchors rather than sibling files.

Read the shared [test policy](../tdd/TEST-POLICY.md) when planning retained automated tests. Record the development stage (default: unstable), agreed seams, and retained scope in the overview; work packages inherit one project-wide budget, ideally at or below 15% and never above 30%. Unstable code retains only representative happy paths. Acceptance criteria still cover all required behavior and name direct or temporary verification where retained tests are out of scope.

## Directory outline

Choose from the optional design topics below according to the requirements.

```text
<topic>/
  README.md
  design/
    README.md
    glossary.md
    architecture.md
    flows.md
    contracts.md
    data.md
    ui.md
    decisions.md
  tasks/
    01-<work-package>.md
    02-<work-package>.md
  acceptance.md
```

Keep shared design in `design/` rather than a catch-all `design.md`. Put local design in sections of the relevant topic. If a topic still contains distinct designs that readers can use independently, split it further by module or subtopic within `design/` and update the design index.

## What each file contains

| File | Content and boundaries | When to generate it |
| --- | --- | --- |
| `README.md` | Goal and user-visible outcome; original requirements, sources, and identifiers; hard constraints and authorized exclusions; complete roadmap, work-package dependencies and parallel work; navigation; links to critical unresolved decisions and their effects. Link to detailed designs without repeating their contents. | Always for a directory plan, as its single overview entry point. |
| `design/README.md` | Design index, the question each file answers, intended readers and reading order; links to existing authoritative project documents; omitted optional topics and why they do not apply. | When there is separate design material; otherwise explain this in the overview. |
| `design/glossary.md` | New or semantically changed domain terms: definitions, scope, relationships, evidence, and status. Stable terms reference the existing project glossary. See the increment rules below. | When there are new terms, changed meanings, or ambiguities worth recording. Otherwise link the existing glossary in the design index without creating an empty file. |
| `design/architecture.md` | Verified current state; affected modules' responsibilities, dependency directions, and main data flow; reuse and necessary additions or changes; useful code locations. Link to the owning files for interface and field details. | When module structure or collaboration needs design. |
| `design/flows.md` | Main user and system flows, state transitions, normal and failure paths; relevant cancellation, retries, timeouts, or concurrency. Define triggers, outcomes, and recovery. | When multistep behavior, state, or failure interactions need explanation. |
| `design/contracts.md` | Inputs, outputs, and semantics of module and external interfaces, events, and messages; validation, errors, permissions, idempotency, and compatibility. Reference reused data entities without copying their field definitions. | When interfaces or collaboration contracts are added or changed. |
| `design/data.md` | Entities and relationships, persisted field meanings and constraints; storage, lifecycle, consistency, and necessary compatibility, backfill, migration, or recovery arrangements. Link to flows for business state-transition rules. | When data models, persistence, or migration behavior are added or changed. |
| `design/ui.md` | Pages and navigation, main interactions, loading, empty and error states, and action feedback; relevant accessibility or adaptation requirements. Reference flows and contracts for business semantics. | When a user interface is involved. |
| `design/decisions.md` | Important choices, evidence, alternatives, and consequences; assumptions affecting the approach and their validation; unresolved options and affected tasks. Reuse valid existing ADRs through links instead of duplicating decisions. | When there are material tradeoffs worth retaining, not routine naming or function details. |
| `tasks/<work-package>.md` | Work-package identifier, deliverable behavior, associated requirements, dependencies, required design links and their purpose, implementation boundaries, local acceptance and its verification method, retained test scope under the shared budget, and relevant risks. Size by assignable and verifiable work, not one file per function. | Per work package in a directory plan. |
| `acceptance.md` | Coverage linking requirements to work packages and acceptance scenarios; final end-to-end integration, regressions, and necessary migration or rollout validation; known check entry points and conditions still to verify. Link to work packages for local acceptance. Distinguish required verification from retained automated tests. | Always for a directory plan; retain an equivalent section in a single-file plan. |

## Glossary increments

Before writing `design/glossary.md`, read the existing project glossary so existing concepts are not renamed or copied into a second set of definitions. Each entry contains at least:

| Item | Required content |
| --- | --- |
| Term and definition | A precise definition of the new concept. For a changed concept, reference the previous definition and explain the difference. |
| Scope and relationships | The behaviors or modules where the definition applies, its relationships to existing concepts, and boundary examples when useful. |
| Evidence and status | The user requirement, verified project fact, or technical choice behind the definition. Label it as established, awaiting confirmation, or awaiting validation; do not present a critical ambiguity as settled fact. |
| Merge arrangement | The authoritative project glossary to update during implementation and the responsible work package. Stable, unchanged terms only reference their existing definitions. |

This file is the single detailed source for the plan's glossary increments. Other plan files use consistent names and link to the entries rather than copying a definition table. Format and protocol fields belong in contracts or data, not in the glossary.

When no project glossary exists, record known terms and evidence without inventing an authoritative project document. Planning alone does not authorize updating the project glossary, writing global ADRs, or changing the product. Put those implementation actions in work packages unless the user has separately authorized them.

## Navigation and single-source maintenance

- The overview owns requirement identifiers and original meanings. Work packages reference those identifiers; overall acceptance references requirements and work packages. Completeness comes from this mapping, not from placing all content in one file.
- Link every detailed design from the design index. Each work package links directly to the design files or sections needed to implement it and explains which constraints they provide. Readers should not have to read all designs first.
- Give each fact, definition, or contract one detailed source. Indexes, roadmaps, and tasks may summarize briefly, but link to the source instead of maintaining copies.
- Keep risks, assumptions, and decision conditions with their relevant topic. The overview links to unresolved items that affect delivery. Splitting must not hide blockers.
- Use resolvable relative links and stable requirement and work-package identifiers. Check index coverage, link targets, identifier uniqueness, dependencies, and requirement coverage. File existence or keyword matches do not establish complete content.
- Explain omitted optional topics in the design index, or in the overview when no separate design directory is needed. Do not create optional files containing only a heading or "none."

## Revise an existing plan

Preserve requirement identifiers, decision evidence, unresolved items, and valid references. Keep an existing structure when it still fits. When converting a long single file to a directory, move its design into `design/` and tasks into `tasks/`, then update the overview, acceptance, and known incoming links. The original entry point may remain as navigation, but must not leave two conflicting full plans with equal authority or silently discard sections that are still referenced.
