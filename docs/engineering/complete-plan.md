## What it does

`complete-plan` turns software requirements into one complete implementation plan. The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) handles the necessary research, ordinary technical choices, work packages, and acceptance coverage instead of sending you through a chain of separate skills.

It plans the entire scope you requested, even when the work needs several phases. Writing a plan is not permission to change product code, install dependencies, create issues, or execute a migration.

## When to reach for it

Type `/complete-plan`, or the agent reaches for it automatically when a task fits.

| What you need | Use |
| --- | --- |
| A software implementation plan, a technical proposal, or requirements organised into executable work packages | `complete-plan` |
| An existing plan repaired so it covers the original requirements | `complete-plan` |
| A deliberate, in-depth interview to stress-test your thinking | [grilling](https://aihero.dev/skills-grilling) |
| Throwaway code to answer one design question | [prototype](https://aihero.dev/skills-prototype) |

## Prerequisites

No issue tracker or setup skill is required. Existing code, tests, and project documents make the plan more specific; when they cannot be inspected, the plan marks assumptions and the checks needed before implementation.

A saved plan follows the project's document convention. With no convention or requested location, it goes under `docs/plans/`. If you ask for the plan only in chat, the response stays in chat.

## Complete delivery, one entry point

A phase schedules work; it does not silently remove requirements. Every requirement needs an implementation path and an acceptance check, and the final integration must join those paths into a usable result. If you explicitly ask only for a prototype, the plan respects that smaller scope rather than adding production infrastructure.

Ordinary technical decisions belong to the agent. It asks you only when an unresolved choice materially changes delivery and no safe default fits the known requirements. An unanswered critical question stays visible as a condition, not an implied approval.

## Common questions

**Does requesting the plan start implementation or create issues?**

No. The default deliverable is one plan document, or its contents in chat when requested. Building the feature, changing product code, or publishing issues needs separate authorization.

**Must I first configure a tracker or run the interview and spec skills?**

No. This is a self-contained planning entry point. You can still choose those workflows when you want them, but they are not prerequisites for getting the complete plan.

## It's working if

- Every original requirement has a work package and an acceptance check.
- The route ends at complete integration, not just the first runnable slice.
- You are not asked to decide facts that the agent could verify or routine internal implementation details.
- Important unknowns are labelled, and unresolved irreversible choices are not presented as approved.
- You receive the plan without product changes, issue creation, or a required hand-off through other skills.

## Where it fits

This is a standalone engineering planning entry point. [grill-with-docs](https://aihero.dev/skills-grill-with-docs) is the neighbour for an interview that also maintains domain documents; [to-spec](https://aihero.dev/skills-to-spec) packages an already-developed conversation for a tracker-based flow. Neither is a required step before this skill. [ask-matt](https://aihero.dev/skills-ask-matt) maps the alternatives.
