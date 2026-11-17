---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Before adding or retaining tests, read the shared [test policy](../tdd/TEST-POLICY.md). Use the documented development stage, defaulting to unstable: happy-path tests only while unstable, a 15% ideal upper bound and a 30% hard limit for the project-wide test-code ratio. Call the Skill tool with "tdd" where possible, at pre-agreed seams within that scope.

Run typechecking regularly, relevant existing test files regularly, and the full existing test suite once at the end. Running a suite does not require adding cases. Remove temporary non-retained reproductions after verifying the fix; do not delete unrelated tests.

Once done, call the Skill tool with "code-review" to review the work.

Commit your work to the current branch.
